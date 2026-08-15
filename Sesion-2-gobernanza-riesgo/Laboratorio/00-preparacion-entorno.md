[🏠 README — Laboratorio 2](README.md) · Sección 0 de 8

## Parte 0 — Preparación del entorno

### 0.1 Verificar la cuenta Free Tier

1. Ingresa a <https://portal.azure.com/> con tu cuenta.
2. En la barra de búsqueda superior escribe `Suscripciones` y haz clic en el resultado **Suscripciones**.
3. Verifica que tu suscripción diga **"Free Trial"**, **"Azure para estudiantes"** o similar. Anota el **ID de suscripción** (una cadena tipo `00000000-0000-0000-0000-000000000000`) — la necesitarás más adelante.

> 📸 **Captura para el informe — 0.1:** la página de Suscripciones mostrando el nombre del plan y el ID de suscripción.

### 0.2 Instalar Azure CLI

1. Descarga el instalador MSI desde: <https://aka.ms/installazurecliwindows>
2. Ejecuta el instalador y sigue el asistente dejando todas las opciones por defecto.
3. Cierra y vuelve a abrir PowerShell (para que reconozca el nuevo comando).
4. Verifica la instalación:

```powershell
az --version
```

Debes ver una salida similar a:

```text
azure-cli                         2.65.0

core                              2.65.0
telemetry                          1.1.0
```

> 📸 **Captura para el informe — 0.2:** la salida completa de `az --version` en tu terminal.

> **Importante:** para la Parte 2 necesitas **Azure CLI 2.61.0 o superior** (soporte de Deployment Stacks). Si tu versión es menor, ejecuta:
>
> ```powershell
> az upgrade
> ```

### 0.3 Instalar Python

1. Descarga Python 3.11 o 3.12 desde <https://www.python.org/downloads/> (Prowler requiere Python ≥ 3.9 y < 3.13).
2. Durante la instalación, **marca la casilla "Add python.exe to PATH"** antes de hacer clic en "Install Now". Este paso es el que más se olvida y causa el error "python no se reconoce como comando".
3. Verifica la instalación abriendo una nueva ventana de PowerShell:

```powershell
python --version
pip --version
```

### 0.4 Instalar Node.js y Git

1. Descarga Node.js LTS desde <https://nodejs.org/en> (elige la versión "LTS", no la "Current").
2. Ejecuta el instalador dejando todas las opciones por defecto.
3. Descarga Git desde <https://git-scm.com/downloads> e instálalo con las opciones por defecto.
4. Verifica ambas instalaciones en una nueva ventana de PowerShell:

```powershell
node --version
npm --version
git --version
```

### 0.5 Iniciar sesión en Azure desde la terminal

```powershell
az login
```

Esto abrirá tu navegador para que inicies sesión con tu cuenta de Azure. Una vez autenticado, la terminal mostrará un JSON con tus suscripciones. Confirma que la propiedad `"isDefault": true` corresponda a la suscripción Free Tier que verificaste en el paso 0.1. Si tienes varias suscripciones y necesitas cambiar la activa:

```powershell
az account set --subscription "<ID-de-tu-suscripción>"
```

Guarda tu ID de suscripción en una variable de PowerShell para reutilizarla durante todo el laboratorio:

```powershell
$SUBSCRIPTION_ID = az account show --query id -o tsv
echo $SUBSCRIPTION_ID
```

> 📸 **Captura para el informe — 0.5:** la terminal mostrando el resultado de `az account show` (o el JSON de `az login`) con `"isDefault": true` en tu suscripción Free Tier.

### 0.6 Crear el grupo de recursos base del laboratorio

Todo lo que creemos vivirá dentro de un único grupo de recursos, para poder borrarlo todo con un solo comando al finalizar.

```powershell
$RG_NAME = "rg-lab2-jvargas"      # reemplaza "jvargas" por tus iniciales
$LOCATION = "eastus"               # puedes usar otra región cercana, p. ej. "brazilsouth"

az group create --name $RG_NAME --location $LOCATION
```

Salida esperada (resumida):

```json
{
  "id": "/subscriptions/xxxx/resourceGroups/rg-lab2-jvargas",
  "location": "eastus",
  "name": "rg-lab2-jvargas",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

✅ **Checkpoint:** en el portal, busca "Grupos de recursos" y confirma que `rg-lab2-jvargas` aparece en la lista.

> 📸 **Captura para el informe — 0.6:** el grupo de recursos `rg-lab2-jvargas` visible en la lista de "Grupos de recursos" del portal.

---

## Preguntas de repaso

1. ¿Por qué este laboratorio insiste en usar un único grupo de recursos para todo lo que se crea?
2. ¿Qué diferencia hay entre el crédito de USD 200 por 30 días y los servicios "Always Free" de Azure?
3. ¿Qué pasaría si ejecutas los comandos de este laboratorio sin haber hecho antes `az login`?
4. Menciona dos motivos por los que instalar Python y Node.js es un requisito antes de llegar a la Parte 4 y la Parte 5.

⬅️ [Inicio (README)](README.md) · 🏠 [README](README.md) · Siguiente ➡️: [Parte 1 · Azure Policy e Initiative](01-azure-policy-initiative.md)
