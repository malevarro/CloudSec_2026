[← Volver al README](README.md) · Sección 6 de 7 · Parte C — Respuesta

# 6. Contención y erradicación

## Objetivo de la sección

Ejecutar, en orden, el playbook de respuesta ante el incidente que detectaste en la Sección 5 — replicando exactamente la tabla "Playbooks de respuesta" del Bloque 3 de la presentación.

---

## 6.1 Confirmar que estás de vuelta en tu sesión de administrador

```bash
az account show --query user
```

Debe mostrar tu cuenta normal, **no** el service principal. Si accidentalmente sigues en la sesión del "atacante", ejecuta `az logout` y vuelve a iniciar sesión con tu cuenta:

```bash
az login
```

---

## 6.2 Paso 1 del playbook — Revocar la asignación de rol de escalación

Elimina el rol `Owner` que el "atacante" se asignó a sí mismo en el paso 4.5:

```bash
az role assignment delete \
  --assignee <appId> \
  --role "Owner" \
  --scope /subscriptions/<TU-SUBSCRIPTION-ID>/resourceGroups/<RG>
```

Verifica que ya no aparece:

```bash
az role assignment list --assignee <appId> --output table
```

Debería mostrar únicamente el rol original `Contributor`.

📸 **Captura para el informe — 6.1**: salida de `az role assignment list` mostrando que el rol `Owner` fue removido.

---

## 6.3 Paso 2 del playbook — Deshabilitar por completo el service principal comprometido

En un incidente real, primero se **deshabilita** la identidad (contención rápida) y luego se decide si se elimina por completo (erradicación). Aquí hacemos ambas, ya que el service principal fue creado únicamente para este ejercicio:

```bash
az ad sp delete --id <appId>
```

Confirma que ya no existe:

```bash
az ad sp show --id <appId>
```

Este comando debe devolver un error (`ResourceNotFound`) — esa es la confirmación correcta.

📸 **Captura para el informe — 6.2**: el comando `az ad sp delete` ejecutado y la confirmación de que `az ad sp show` ya no encuentra el service principal.

---

## 6.4 Paso 3 del playbook — Rotar los secretos accedidos

El "atacante" leyó al menos un secreto del Key Vault en el paso 4.5 (Acción 1). En un incidente real, cualquier secreto al que haya tenido acceso una identidad comprometida se considera **potencialmente filtrado** y debe rotarse.

```bash
az keyvault secret set \
  --vault-name <nombre-keyvault> \
  --name <nombre-del-secreto-accedido> \
  --value "<nuevo-valor-generado>"
```

> 💡 En un entorno productivo, el nuevo valor normalmente se genera automáticamente (por ejemplo, una nueva contraseña de base de datos) y se actualiza también en la aplicación que lo consume — no basta con cambiarlo solo en el Key Vault.

📸 **Captura para el informe — 6.3**: confirmación de la nueva versión del secreto en el Key Vault (Portal → Key Vault → Secrets → el secreto → verás una nueva versión con fecha reciente).

---

## 6.5 Paso 4 del playbook — Aislar el recurso expuesto (demostración de contención de red)

Aunque en este escenario el compromiso fue de identidad y no de red, un incidente real suele requerir también contención a nivel de red. Practica el patrón con una regla NSG temporal sobre la subred donde corre DVWA:

```bash
az network nsg rule create \
  --resource-group <RG> \
  --nsg-name <nombre-nsg-spoke-app> \
  --name deny-temporal-investigacion \
  --priority 100 \
  --direction Inbound \
  --access Deny \
  --protocol "*" \
  --source-address-prefixes "*" \
  --destination-address-prefixes "*" \
  --destination-port-ranges "*"
```

> Esta regla, con prioridad `100`, se evalúa antes que cualquier otra regla del NSG — bloquea todo el tráfico entrante hacia esa subred mientras se completa la investigación, exactamente el mismo principio de microsegmentación *deny-by-default* que viste en la Sesión 4.

🧪 **Checkpoint 6.A**: verifica que `http://<IP-PUBLICA-AGW>` ya no carga DVWA mientras esta regla existe.

📸 **Captura para el informe — 6.4**: intento de acceso a DVWA fallando (timeout o error de conexión) con la regla de aislamiento activa.

### Revertir el aislamiento (una vez "investigado" el incidente)

```bash
az network nsg rule delete \
  --resource-group <RG> \
  --nsg-name <nombre-nsg-spoke-app> \
  --name deny-temporal-investigacion
```

---

## 6.6 Verificación final de erradicación

Ejecuta esta lista de comprobación y documenta el resultado de cada uno en tu informe:

```bash
# 1. El service principal ya no existe
az ad sp show --id <appId>

# 2. No quedan asignaciones de rol asociadas a ese appId
az role assignment list --assignee <appId> --output table

# 3. El secreto tiene una versión más reciente que la hora del incidente
az keyvault secret list-versions --vault-name <nombre-keyvault> --name <nombre-del-secreto-accedido> --output table
```

📸 **Captura para el informe — 6.5**: los tres resultados de verificación, confirmando la erradicación completa.

---

## 🧩 Preguntas de repaso — Sección 6

1. ¿Por qué el playbook revoca primero la asignación de rol (6.2) y **después** elimina el service principal (6.3), en lugar del orden inverso?
2. En un incidente real donde el service principal tuviera credenciales válidas para múltiples recursos, ¿por qué no basta con rotar solo el secreto que se confirmó accedido (6.4)?
3. Explica la diferencia entre "contención" (6.5) y "erradicación" (6.2-6.3) usando los pasos concretos de esta sección como ejemplo de cada una.
4. La regla NSG del paso 6.5 usa `priority 100`. ¿Qué pasaría si en la subred ya existiera otra regla con `priority 90` que permitiera todo el tráfico? Justifica tu respuesta con lo aprendido sobre NSG en la Sesión 4.

---

[← Anterior: 5. Detección con KQL](05-Deteccion-KQL.md) · [← Volver al README](README.md) · Siguiente: [7. RCA, informe ejecutivo y limpieza final →](07-RCA-e-Informe-Ejecutivo.md)
