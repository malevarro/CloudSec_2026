[← Volver al README](README.md) · Sección 2 de 7 · Parte A — Visibilidad

# 2. Habilitar visibilidad: Log Analytics, Diagnostic Settings y Flow Logs

## Objetivo de la sección

Construir el pipeline de telemetría que usarán las secciones 4 y 5 para detectar el incidente simulado. Al terminar, tendrás **un único Log Analytics Workspace** recibiendo: logs de red (Flow Logs), logs de Key Vault y Storage (data plane), logs de administración de la suscripción (Activity Log) y — gracias al trial de Entra ID P1 — **logs de inicio de sesión (Sign-in Logs)**.

---

## 2.1 Crear el Log Analytics Workspace

### Por el Portal

1. En el buscador superior del portal, escribe **"Log Analytics workspaces"** y selecciónalo.
2. Clic en **+ Create**.
3. En la pestaña **Basics**:
   - **Subscription**: tu suscripción.
   - **Resource group**: `<RG>` (el mismo de la Sesión 4).
   - **Name**: `law-seguridad-nube-s5`.
   - **Region**: la misma región donde están tus recursos de la Sesión 4 (por ejemplo, `East US` o `Brazil South` — deben coincidir para evitar cargos de transferencia entre regiones).
4. Clic en **Review + create**, espera la validación en verde, y clic en **Create**.
5. Espera 1-2 minutos hasta que el despliegue termine (notificación de campana en la esquina superior derecha).

### O por CLI (equivalente, más rápido)

```bash
az monitor log-analytics workspace create \
  --resource-group <RG> \
  --workspace-name law-seguridad-nube-s5 \
  --location <tu-region>
```

📸 **Captura para el informe — 2.1**: página **Overview** del Log Analytics Workspace recién creado, mostrando su nombre y región.

---

## 2.2 Habilitar Diagnostic Settings en Key Vault

1. Ve a tu **Key Vault** de la Sesión 3/4.
2. En el menú izquierdo, bajo **Monitoring**, clic en **Diagnostic settings**.
3. Clic en **+ Add diagnostic setting**.
4. Nombre: `diag-keyvault-a-law`.
5. En **Categories**, marca:
   - [x] `AuditEvent` (registra cada GET/SET de secretos y llaves).
6. En **Destination details**, marca **Send to Log Analytics workspace** y selecciona `law-seguridad-nube-s5`.
7. Clic en **Save**.

📸 **Captura para el informe — 2.2**: configuración de Diagnostic Settings del Key Vault, mostrando `AuditEvent` marcado y el workspace de destino.

---

## 2.3 Habilitar Diagnostic Settings en la Storage Account

Repite el mismo procedimiento en tu **Storage Account**:

1. Ve a la Storage Account → **Diagnostic settings** (bajo **Monitoring**).
2. Si tu Storage Account tiene sub-servicios (Blob, Table, Queue), el diagnóstico se configura **por sub-servicio**. Entra a **Blob service** dentro del menú de la Storage Account → **Diagnostic settings**.
3. **+ Add diagnostic setting** → nombre `diag-storage-a-law`.
4. Marca las categorías `StorageRead`, `StorageWrite` y `StorageDelete`.
5. Destino: **Send to Log Analytics workspace** → `law-seguridad-nube-s5`.
6. **Save**.

---

## 2.4 Habilitar Flow Logs y Traffic Analytics (retomando la Sesión 4)

Ya conoces este componente — es el mismo que viste en la Sesión 4 ("Visibilidad: no se microsegmenta a ciegas"). Hoy lo conectamos al workspace central.

1. En el buscador, escribe **"Network Watcher"** y ábrelo.
2. En el menú izquierdo, bajo **Logs**, clic en **Flow logs**.
3. Clic en **+ Create**.
4. **Target resource type**: `Network Security Group`.
5. Selecciona el NSG de tu spoke de aplicación (el que protege `VM-App` / la subred donde irá DVWA).
6. **Flow logs settings**: activa el flow log, retención en `7` días (suficiente para este laboratorio y dentro de lo gratuito).
7. **Traffic Analytics**: actívalo y selecciona el mismo `law-seguridad-nube-s5` como workspace de destino.
8. **Review + create** → **Create**.

📸 **Captura para el informe — 2.3**: configuración del Flow Log con Traffic Analytics apuntando al workspace de la Sesión 5.

> 💡 Traffic Analytics puede tardar hasta 60 minutos en mostrar el primer procesamiento de datos — es normal no ver nada inmediatamente en las consultas.

---

## 2.5 Habilitar el Activity Log de la suscripción hacia el workspace

El Activity Log ya existe de forma gratuita y automática (lo viste en la Sesión 2), pero por defecto **no** llega a Log Analytics — hay que enrutarlo explícitamente.

1. En el buscador, escribe **"Activity log"** y ábrelo (es un servicio a nivel de suscripción, no de un recurso puntual).
2. Clic en **Export Activity Logs** (en el menú superior o lateral, según la versión del portal).
3. Clic en **+ Add diagnostic setting**.
4. Nombre: `diag-activitylog-a-law`.
5. Marca todas las categorías (`Administrative`, `Security`, `Policy`, al menos).
6. Destino: **Send to Log Analytics workspace** → `law-seguridad-nube-s5`.
7. **Save**.

---

## 2.6 Activar el trial gratuito de Microsoft Entra ID P1

Este paso es indispensable para poder exportar los **Sign-in Logs** hacia Log Analytics — sin licencia P1 o P2, Azure bloquea esa exportación específica, incluso con Log Analytics ya configurado.

1. Ve a **https://entra.microsoft.com** e inicia sesión con la misma cuenta (debes tener el rol **Global Administrator** sobre el tenant).
2. En el menú izquierdo, ve a **Billing → Licenses → All products**.
3. Clic en **+ Try/Buy products**.
4. Busca **"Microsoft Entra ID P1"** y selecciona la opción de **prueba gratuita (Free trial)** de 30 días.
5. Confirma la activación. La licencia se activa de inmediato (puede tardar unos minutos en reflejarse).

📸 **Captura para el informe — 2.4**: pantalla de **Licenses** mostrando "Microsoft Entra ID P1" activo como trial.

> ⚠️ Este trial es **gratuito y de un solo uso por tenant**. No lo actives más de una vez ni lo uses fuera del contexto de este laboratorio — es un recurso educativo, no una licencia productiva.

---

## 2.7 Exportar Sign-in Logs y Audit Logs de Entra ID hacia el workspace

1. En el mismo portal de Entra (**entra.microsoft.com**), ve a **Identity → Monitoring & health → Diagnostic settings**.
2. Clic en **+ Add diagnostic setting**.
3. Nombre: `diag-entra-a-law`.
4. Marca las categorías:
   - [x] `SignInLogs`
   - [x] `AuditLogs`
5. Destino: **Send to Log Analytics workspace** → suscripción y `law-seguridad-nube-s5`.
6. **Save**.

📸 **Captura para el informe — 2.5**: configuración de Diagnostic Settings de Entra ID con `SignInLogs` y `AuditLogs` marcados.

---

## 2.8 Verificar que los datos están llegando

Los distintos orígenes tardan tiempos distintos en aparecer (de 5 a 30 minutos). Ve a tu Log Analytics Workspace → **Logs** y ejecuta, una por una, estas consultas de verificación:

```kql
AzureActivity
| take 10
```

```kql
SigninLogs
| take 10
```

```kql
AzureDiagnostics
| where ResourceType == "VAULTS"
| take 10
```

🧪 **Checkpoint 2.A**: no continúes a la Sección 3 hasta que al menos `AzureActivity` y `SigninLogs` devuelvan resultados. Si `SigninLogs` sigue vacío después de 30 minutos, vuelve al paso 2.6 y confirma que la licencia P1 quedó activa (Sección 2.6) y que el diagnostic setting del paso 2.7 se guardó correctamente.

📸 **Captura para el informe — 2.6**: resultado de la consulta `SigninLogs | take 10` mostrando al menos una fila.

---

## 🧩 Preguntas de repaso — Sección 2

1. ¿Por qué el Activity Log y los Diagnostic Logs de un recurso específico (como el Key Vault) requieren pasos de configuración distintos, aunque ambos terminen en el mismo workspace?
2. Explica, en tus palabras, por qué exportar `SigninLogs` requirió activar una licencia P1 de Entra ID mientras que exportar los logs del Key Vault no la requirió.
3. ¿Qué diferencia hay entre "Flow Logs" y "Traffic Analytics", y por qué se configuran juntos en este laboratorio?
4. Si tuvieras que priorizar solo tres fuentes de datos para un presupuesto de Log Analytics limitado, ¿cuáles elegirías para este escenario de compromiso de credenciales IAM, y por qué?

---

[← Anterior: 1. Preparación y reactivación](01-Preparacion-y-Reactivacion.md) · [← Volver al README](README.md) · Siguiente: [3. Despliegue de WAF y DVWA →](03-Despliegue-WAF-DVWA.md)
