[← Volver al README](README.md) · Sección 5 de 7 · Parte B — Detección

# 5. Detección con KQL y mapeo a MITRE ATT&CK for Cloud

## Objetivo de la sección

Reconstruir, **usando únicamente los logs** (no tu memoria del paso anterior), lo que hizo el "atacante" en la Sección 4. Cada consulta que ejecutes aquí corresponde a una fila de la tabla de la diapositiva "Caso aplicado" del Bloque 2 de la presentación.

> 💡 Espera al menos 15 minutos después de terminar la Sección 4 antes de ejecutar estas consultas — los distintos orígenes de log tardan tiempos distintos en llegar a Log Analytics.

En el portal, ve a tu **Log Analytics Workspace** (`law-seguridad-nube-s5`) → **Logs**, y ejecuta cada consulta en el editor.

---

## 5.1 Detectar el inicio de sesión del service principal (Credential Access)

```kql
SigninLogs
| where TimeGenerated > ago(2h)
| where AppDisplayName == "sp-dvwa-deploy-lab" or ServicePrincipalName == "sp-dvwa-deploy-lab"
| project TimeGenerated, ServicePrincipalName, IPAddress, ResultType, Location
| order by TimeGenerated asc
```

> Si el nombre no aparece exactamente así, prueba buscando por el `appId` que anotaste en la Sección 4:
> ```kql
> SigninLogs
> | where TimeGenerated > ago(2h)
> | where AppId == "<tu-appId>"
> | project TimeGenerated, AppId, IPAddress, ResultType, Location
> ```

📸 **Captura para el informe — 5.1**: resultado de esta consulta mostrando el inicio de sesión del service principal.

**Mapeo MITRE ATT&CK**: *Credential Access* → uso de credenciales válidas obtenidas fuera de banda.

---

## 5.2 Detectar la escalación de privilegios (Privilege Escalation)

```kql
AzureActivity
| where TimeGenerated > ago(2h)
| where OperationNameValue has "MICROSOFT.AUTHORIZATION/ROLEASSIGNMENTS/WRITE"
| project TimeGenerated, Caller, OperationNameValue, ActivityStatusValue, ResourceGroup
| order by TimeGenerated asc
```

Busca la fila donde `Caller` coincide con el `appId` de tu service principal — esa es la operación "Create role assignment" que ejecutaste en el paso 4.5 (Acción 2).

📸 **Captura para el informe — 5.2**: resultado mostrando la operación de creación del role assignment, con el `Caller` resaltado.

**Mapeo MITRE ATT&CK**: *Privilege Escalation* → ampliación de permisos de la identidad comprometida de `Contributor` a `Owner`.

---

## 5.3 Detectar el acceso a secretos del Key Vault

```kql
AzureDiagnostics
| where TimeGenerated > ago(2h)
| where ResourceType == "VAULTS"
| where OperationName in ("SecretGet", "SecretList")
| project TimeGenerated, OperationName, CallerIPAddress, identity_claim_appid_g, ResultType
| order by TimeGenerated asc
```

Identifica las filas donde `identity_claim_appid_g` coincide con tu service principal — corresponden a la Acción 1 del paso 4.5.

📸 **Captura para el informe — 5.3**: resultado mostrando los accesos `SecretGet`/`SecretList` del service principal comprometido.

**Mapeo MITRE ATT&CK**: *Credential Access* (continuación) → recolección de secretos adicionales una vez dentro del entorno.

---

## 5.4 Reconstruir la línea de tiempo completa

Ahora construye una sola consulta que una las tres fuentes en una línea de tiempo, ordenada cronológicamente:

```kql
let inicio = SigninLogs
    | where TimeGenerated > ago(2h)
    | where AppId == "<tu-appId>"
    | project TimeGenerated, Fuente = "SigninLogs", Evento = strcat("Inicio de sesión desde ", IPAddress), Resultado = ResultType;
let escalacion = AzureActivity
    | where TimeGenerated > ago(2h)
    | where Caller == "<tu-appId>"
    | project TimeGenerated, Fuente = "AzureActivity", Evento = OperationNameValue, Resultado = ActivityStatusValue;
let secretos = AzureDiagnostics
    | where TimeGenerated > ago(2h)
    | where ResourceType == "VAULTS" and identity_claim_appid_g == "<tu-appId>"
    | project TimeGenerated, Fuente = "AzureDiagnostics (KeyVault)", Evento = OperationName, Resultado = ResultType;
inicio
| union escalacion, secretos
| order by TimeGenerated asc
```

Esta es, literalmente, la **línea de tiempo del incidente** que irá en tu informe ejecutivo (Sección 7).

📸 **Captura para el informe — 5.4**: resultado completo de la consulta unificada, mostrando los eventos en orden cronológico.

🧪 **Checkpoint 5.A**: antes de continuar, confirma que puedes explicar, sin ver tus notas de la Sección 4, qué ocurrió y en qué orden — usando solo esta tabla.

---

## 5.5 Completar la tabla de mapeo a MITRE ATT&CK

Con lo detectado en los pasos 5.1 a 5.4, completa esta tabla en tu informe (rellena la columna de la derecha con lo que observaste tú, con tus propios valores de IP/hora):

| Táctica ATT&CK | Técnica observada | Evidencia encontrada (tu consulta y resultado) |
|---|---|---|
| Credential Access | Uso de credenciales filtradas de un service principal | *(completa con tu resultado de 5.1)* |
| Privilege Escalation | Asignación de rol Owner a la identidad comprometida | *(completa con tu resultado de 5.2)* |
| Credential Access | Enumeración y lectura de secretos en Key Vault | *(completa con tu resultado de 5.3)* |

---

## 🧩 Preguntas de repaso — Sección 5

1. ¿Por qué la consulta del paso 5.2 filtra por `MICROSOFT.AUTHORIZATION/ROLEASSIGNMENTS/WRITE` en lugar de simplemente buscar la palabra "Owner" en todo el Activity Log?
2. En la línea de tiempo unificada (5.4), ¿qué evento ocurrió primero: el inicio de sesión, el acceso a secretos o la escalación de privilegios? ¿Ese orden coincide con la secuencia lógica de un ataque real? Explica por qué.
3. Si no hubieras tenido la licencia de Entra ID P1 activa, ¿cuál de las tres consultas de esta sección habría sido imposible de ejecutar, y qué alternativa (aunque más limitada) tendrías para investigar ese mismo evento?
4. ¿Qué campo de `SigninLogs` revisarías primero para distinguir un inicio de sesión legítimo de uno sospechoso, si no tuvieras de antemano el `appId` exacto del atacante?

---

[← Anterior: 4. Simulación del incidente](04-Simulacion-Incidente.md) · [← Volver al README](README.md) · Siguiente: [6. Contención y erradicación →](06-Contencion-y-Erradicacion.md)
