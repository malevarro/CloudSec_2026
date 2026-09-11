[← Volver al README](README.md) · Sección 4 de 7 · Parte B — Detección

# 4. Simulación controlada de un compromiso de credenciales IAM

## Objetivo de la sección

Generar, de forma **segura, controlada y reproducible**, la evidencia de un incidente real de seguridad en tu propio entorno de laboratorio — para que en la Sección 5 puedas practicar la detección con datos auténticos en lugar de un caso de estudio en papel.

> ⚠️ **Uso responsable**: todo lo que hagas en esta sección ocurre exclusivamente dentro de **tu propia suscripción y tenant de laboratorio**. El objetivo es generar telemetría defensiva para aprender a detectarla — no es una técnica para usar contra sistemas de terceros ni fuera de este ejercicio académico. Al finalizar la sección, el service principal creado se elimina por completo (Sección 6).

---

## 4.1 El escenario (contexto ficticio)

> *Un desarrollador del equipo de la aplicación DVWA necesitaba automatizar despliegues y creó un service principal con permisos de Contributor sobre el resource group. Por error, subió el archivo con el `appId` y el `password` a un repositorio de código que quedó configurado como público durante unas horas. Un actor externo lo encontró y lo utilizó para acceder a la suscripción.*

Esto es exactamente el patrón "Credential Access" que viste en el Bloque 2 de la presentación (MITRE ATT&CK for Cloud) — y es, según múltiples reportes de la industria, una de las causas más comunes de incidentes reales en la nube.

---

## 4.2 Crear el service principal "filtrado"

Desde Cloud Shell, con tu cuenta normal (administrador del laboratorio):

```bash
az ad sp create-for-rbac \
  --name "sp-dvwa-deploy-lab" \
  --role Contributor \
  --scopes /subscriptions/<TU-SUBSCRIPTION-ID>/resourceGroups/<RG>
```

Reemplaza `<TU-SUBSCRIPTION-ID>` por el valor que obtienes con `az account show --query id --output tsv`.

La salida se ve así (los valores reales serán distintos):

```json
{
  "appId": "11111111-2222-3333-4444-555555555555",
  "displayName": "sp-dvwa-deploy-lab",
  "password": "AbCdEfGhIjKlMnOpQrStUvWxYz123456",
  "tenant": "66666666-7777-8888-9999-000000000000"
}
```

**Copia estos tres valores (`appId`, `password`, `tenant`) en un bloc de notas temporal** — los necesitas en el paso 4.4. Esto simula el archivo que "quedó expuesto en el repositorio público".

📸 **Captura para el informe — 4.1**: salida de `az ad sp create-for-rbac` (puedes tapar visualmente el `password` real en la captura del informe, pero consérvalo para tu propio uso durante el laboratorio).

---

## 4.3 (Opcional pero recomendado) Registrar la hora exacta

Anota la hora UTC actual — te servirá en la Sección 5 para ubicar el incidente en los logs:

```bash
date -u
```

📝 Anota este valor en tu bloc de notas: `Hora de creación del SP: ____________`.

---

## 4.4 Simular al atacante: iniciar sesión con el service principal

Abre una **segunda pestaña** de Cloud Shell (o una ventana de terminal local con Azure CLI instalado) — esto representa la sesión del "atacante", distinta de tu sesión de administrador.

```bash
az login --service-principal \
  --username <appId> \
  --password <password> \
  --tenant <tenant>
```

Confirma que la sesión quedó activa como el service principal:

```bash
az account show --query user
```

Debe mostrar `"type": "servicePrincipal"` y el `appId` que copiaste.

📸 **Captura para el informe — 4.2**: salida de `az account show` confirmando la sesión activa como el service principal.

---

## 4.5 Ejecutar las acciones sospechosas del "atacante"

Con la sesión del service principal activa, ejecuta **en orden** estas tres acciones. Cada una corresponde a una técnica distinta de MITRE ATT&CK for Cloud que mapearás en la Sección 5.

**Acción 1 — Reconocimiento y acceso a secretos (Credential Access):**

```bash
az keyvault secret list --vault-name <nombre-keyvault> --output table
az keyvault secret show --vault-name <nombre-keyvault> --name <nombre-de-un-secreto-existente>
```

**Acción 2 — Escalación de privilegios (Privilege Escalation):**

```bash
az role assignment create \
  --assignee <appId> \
  --role "Owner" \
  --scope /subscriptions/<TU-SUBSCRIPTION-ID>/resourceGroups/<RG>
```

> Esto simula al atacante ampliando su propio acceso de `Contributor` a `Owner` — un patrón clásico de escalación una vez dentro del entorno.

**Acción 3 — Reconocimiento de almacenamiento (preparación de exfiltración):**

```bash
az storage account keys list \
  --resource-group <RG> \
  --account-name <nombre-storage-account>
```

📸 **Captura para el informe — 4.3**: las tres acciones ejecutadas con sus salidas visibles en la terminal.

---

## 4.6 Cerrar la sesión simulada del atacante

```bash
az logout
```

Vuelve a tu primera pestaña de Cloud Shell (tu sesión de administrador normal) para continuar con la Sección 5.

---

## 🧪 Checkpoint 4.A

Antes de avanzar, confirma que tienes anotados en tu bloc de notas:

- [ ] El `appId` del service principal.
- [ ] La hora UTC aproximada en la que ejecutaste el login del "atacante" (paso 4.4).
- [ ] Las tres acciones ejecutadas y el orden en que ocurrieron.

Esta información es la que reconstruirás — como si no la conocieras de antemano — al hacer la detección en la Sección 5.

---

## 🧩 Preguntas de repaso — Sección 4

1. ¿Por qué el escenario asigna al service principal el rol `Contributor` inicial, y no `Owner` desde el principio? ¿Qué le permite hacer la Acción 2 que no podría hacer solo con `Contributor`?
2. Explica la diferencia entre `az login` con un usuario normal y `az login --service-principal`. ¿Por qué los atacantes reales suelen preferir comprometer identidades de aplicación (service principals) en lugar de cuentas de usuario?
3. ¿Qué otras dos técnicas de MITRE ATT&CK for Cloud (distintas a las tres simuladas aquí) podría haber ejecutado un atacante real con acceso `Owner` sobre este resource group?
4. Este ejercicio simula el incidente en un entorno controlado. Menciona dos controles preventivos (vistos en sesiones anteriores del curso) que, de haber estado implementados, habrían evitado o dificultado este escenario.

---

[← Anterior: 3. Despliegue de WAF y DVWA](03-Despliegue-WAF-DVWA.md) · [← Volver al README](README.md) · Siguiente: [5. Detección con KQL y mapeo a MITRE ATT&CK →](05-Deteccion-KQL.md)
