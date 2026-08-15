[🏠 README — Laboratorio 2](README.md) · Sección 1 de 8

## Parte 1 — Azure Policy e Initiative

### 1.1 Conceptos clave

| Término | Definición simple |
| --- | --- |
| **Policy Definition** | Una única regla ("todo Storage Account debe exigir HTTPS"). |
| **Initiative (Policy Set)** | Un grupo de varias Policy Definitions relacionadas, tratadas como una unidad ("línea base de seguridad"). Equivale a un mini-CCM propio. |
| **Assignment** | Aplicar una Policy o Initiative a un ámbito (scope): un grupo de administración, una suscripción o un grupo de recursos. |
| **Effect** | Qué hace la política al evaluar un recurso no conforme: `Audit` (solo reporta), `Deny` (bloquea la creación), `Modify`/`DeployIfNotExists` (corrige automáticamente). |
| **Compliance state** | El resultado de evaluar todos los recursos del scope contra la política asignada: `Compliant`, `Non-compliant`, `Exempt`. |

> Conexión con la teoría: esto es exactamente **Security by Design** aplicado con una herramienta — en vez de confiar en que cada persona configure bien cada recurso, la plataforma **impide o corrige automáticamente** la configuración insegura.

### 1.2 Explorar policies incorporadas (built-in)

Azure trae cientos de políticas ya escritas. Antes de crear una propia, exploremos el catálogo:

1. En el portal, busca **"Policy"** en la barra superior y entra al servicio **Azure Policy**.
2. En el menú izquierdo, haz clic en **Definitions**.
3. En el filtro **Type**, selecciona **Built-in**.
4. En el buscador, escribe `storage` y observa cuántas políticas ya existen para Storage Accounts (por ejemplo: *"Storage accounts should have infrastructure encryption"*, *"Secure transfer to storage accounts should be enabled"*).

<p align="center"><em>[Captura sugerida: lista filtrada de policies built-in relacionadas con "storage"]</em></p>

### 1.3 Crear una Policy Definition personalizada

Vamos a crear una política simple que **audite** (no bloquee, para no complicarnos en el laboratorio) que todo recurso tenga la etiqueta (`tag`) `ambiente`.

1. En **Azure Policy → Definitions**, haz clic en **+ Policy definition** (parte superior).
2. Completa el formulario:
   - **Definition location:** haz clic en los `...` y selecciona tu suscripción Free Tier (no un Management Group, para mantenerlo simple).
   - **Name:** `lab2-audit-tag-ambiente`
   - **Description:** `Audita que todos los recursos tengan la etiqueta 'ambiente'.`
   - **Category:** selecciona **Use existing** → `Tags`.
3. En el cuadro **Policy rule**, borra el contenido de ejemplo y pega lo siguiente:

```json
{
  "mode": "Indexed",
  "policyRule": {
    "if": {
      "field": "tags['ambiente']",
      "exists": "false"
    },
    "then": {
      "effect": "audit"
    }
  }
}
```

4. Haz clic en **Save**.

**Qué acabas de hacer:** definiste una regla declarativa (no un script) que dice *"si el recurso no tiene la etiqueta `ambiente`, repórtalo como no conforme"*. El campo `"effect": "audit"` es intencional — en producción usarías `"deny"`, pero en un laboratorio de aprendizaje `audit` te deja ver el resultado sin bloquear tus propias pruebas.

### 1.4 Crear una Initiative (Policy Set) con varias políticas

Ahora agrupemos nuestra política personalizada junto con dos políticas built-in relevantes para gobernanza, en una sola Initiative.

1. En **Azure Policy**, ve al menú izquierdo → **Definitions** → **+ Policy set definition** (Initiative).
2. Completa:
   - **Definition location:** tu suscripción.
   - **Name:** `ini-lab2-jvargas-baseline`
   - **Description:** `Línea base de gobernanza del Laboratorio 2: etiquetado y protección básica de datos.`
   - **Category:** `Custom`.
3. En la pestaña **Policies**, haz clic en **Add policy definition(s)** y agrega, una por una (usa el buscador):
   - Tu política `lab2-audit-tag-ambiente` (categoría Tags).
   - La política built-in **"Secure transfer to storage accounts should be enabled"** (categoría Storage).
   - La política built-in **"Storage accounts should restrict network access"** (categoría Storage).
4. Haz clic en **Add** y luego en **Save**.

✅ **Checkpoint:** en **Definitions**, filtra por **Type = Custom** y confirma que ves tu Initiative `ini-lab2-jvargas-baseline` con **3 policies** agrupadas.

### 1.5 Asignar (Assign) la Initiative

Una definición no hace nada hasta que se **asigna** a un scope.

1. Abre tu Initiative `ini-lab2-jvargas-baseline` (búscala en **Definitions**).
2. Haz clic en **Assign**.
3. En la pestaña **Basics**:
   - **Scope:** haz clic en los `...`, selecciona tu suscripción y luego tu grupo de recursos `rg-lab2-jvargas` (así limitamos el impacto solo a nuestro laboratorio, no a toda la suscripción).
   - **Assignment name:** se autocompleta con el nombre de la Initiative; déjalo así.
4. Deja las demás pestañas (**Parameters**, **Remediation**, **Non-compliance messages**) con sus valores por defecto.
5. Haz clic en **Review + create** y luego en **Create**.

**Equivalente en Azure CLI** (opcional, si prefieres automatizar):

```powershell
# Obtener el ID completo de la definición de la Initiative
$INITIATIVE_ID = az policy set-definition show --name "ini-lab2-jvargas-baseline" --query id -o tsv

# Asignarla al grupo de recursos del laboratorio
az policy assignment create `
  --name "asg-lab2-baseline" `
  --policy-set-definition $INITIATIVE_ID `
  --scope "/subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RG_NAME"
```

### 1.6 Verificar el cumplimiento (Compliance)

> ⏱️ **Nota de tiempos:** Azure Policy no evalúa en tiempo real de forma instantánea — el primer ciclo de evaluación puede tardar **entre 5 y 30 minutos**. Aprovecha este tiempo para avanzar a la Parte 2 y vuelve aquí después.

1. En **Azure Policy**, ve a **Compliance** en el menú izquierdo.
2. Busca tu asignación `ini-lab2-jvargas-baseline` (o `asg-lab2-baseline` si la creaste por CLI).
3. Haz clic sobre ella para ver el detalle: verás cada política individual y su porcentaje de cumplimiento.

Como el grupo de recursos `rg-lab2-jvargas` está prácticamente vacío en este punto, es normal ver **"No resources found"** o 100 % de cumplimiento — lo interesante viene en el siguiente paso.

### 1.7 Probar la política: forzar un incumplimiento a propósito

Vamos a crear un recurso simple **sin** la etiqueta `ambiente` para comprobar que Azure Policy lo detecta.

```powershell
az storage account create `
  --name "stlab2jvargas" `
  --resource-group $RG_NAME `
  --location $LOCATION `
  --sku Standard_LRS `
  --kind StorageV2
```

> Si el nombre `stlab2jvargas` ya está en uso (los Storage Accounts son únicos **globalmente** en todo Azure), agrega un número al final, p. ej. `stlab2jvargas01`.

Este comando, además, nos sirve para la Parte 3 y 4 más adelante. Espera unos 15-20 minutos y vuelve a la vista de **Compliance** (paso 1.6): deberías ver el Storage Account marcado como **Non-compliant** frente a la política de la etiqueta `ambiente` (porque no la definimos al crearlo) — exactamente el comportamiento esperado.

<p align="center"><em>[Captura sugerida: panel de Compliance mostrando el recurso "Non-compliant"]</em></p>

---

⬅️ Anterior: [Parte 0 · Preparación del entorno](00-preparacion-entorno.md) · 🏠 [README](README.md) · Siguiente ➡️: [Parte 2 · Landing Zone básica con Deployment Stacks](02-landing-zone-deployment-stacks.md)
