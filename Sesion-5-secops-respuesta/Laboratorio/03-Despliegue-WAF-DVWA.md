[← Volver al README](README.md) · Sección 3 de 7 · Parte B — Detección

# 3. Despliegue del WAF y DVWA (el componente diferido de la Sesión 4)

## Objetivo de la sección

Desplegar el componente que quedó pendiente al cerrar la Sesión 4: una aplicación deliberadamente vulnerable (**DVWA — Damn Vulnerable Web Application**) protegida por un **Application Gateway con Web Application Firewall (WAF_v2)**. El tráfico seguirá la misma ruta que en la Sesión 4: **Internet → Application Gateway (WAF) → backend privado**.

A diferencia de ir directo a bloquear, esta sección te hace **probar el mismo ataque dos veces**: primero con la política en modo **Detection** (para ver que el WAF observa pero deja pasar el ataque) y después en modo **Prevention** (para ver el bloqueo). Es la forma más clara de demostrar, con evidencia propia, qué aporta realmente un WAF.

> 💰 **Aviso de costo**: Application Gateway, ACI y el Azure Container Registry que crearás en esta sección **no se pueden apagar** como una VM — cobran por hora/día mientras existen. La Sección 7 termina con su eliminación explícita. No dejes estos recursos corriendo entre sesiones de trabajo.

---

## 3.0 Variables de esta sección

Para mantener los comandos legibles y evitar errores de copiar/pegar, define estas variables una sola vez al inicio de tu sesión de Cloud Shell. Sustituye los valores entre `< >` por los tuyos.

```bash
export RG="<RG>"                        # el resource group de la Sesión 4 (Sección 1)
export LOC="<tu-region>"                # la misma región de tus recursos de la Sesión 4
export SUB_ID=$(az account show --query id -o tsv)
export HUB="<nombre-vnet-hub>"
export SPOKE_APP="<nombre-vnet-spoke-app>"

export ACR="acrdvwas5<tus-iniciales>"   # nombre GLOBALMENTE único — solo minúsculas y números
export APPGW="agw-dvwa-s5"
export WAF_POLICY="wafpol-dvwa-s5"
export APPGW_SUBNET="AppGatewaySubnet"
export ACI_SUBNET="ACISubnet"
```

> 📝 Estas variables solo viven mientras la pestaña de Cloud Shell siga abierta. Si la cierras y vuelves más tarde, tendrás que definirlas de nuevo — por eso cada paso, además de usar la variable, te pide anotar el valor real en tu bloc de notas.

---

## 3.1 Crear un Azure Container Registry propio y evitar el límite de Docker Hub

**Por qué este paso es necesario:** Docker Hub aplica límites de descarga por IP a las cuentas anónimas (10 *pulls*/hora aproximadamente, según la política vigente de Docker). Como muchas suscripciones de Azure salen a Internet por rangos de IP compartidos con otros usuarios del datacenter, **Azure Container Instances puede fallar al descargar `vulnerables/web-dvwa` directamente de Docker Hub** con un error `toomanyrequests` — el cupo ya estaba consumido por tráfico de otros inquilinos antes de que tú lo intentaras.

La solución recomendada por Microsoft es **no depender de Docker Hub en el momento del despliegue**: importas la imagen **una sola vez** a tu propio Azure Container Registry, y desde ahí ACI la descarga sin límite ni dependencia externa.

```bash
# 1. Crear el registro (SKU Basic — el más económico, suficiente para este laboratorio)
az acr create \
  --resource-group $RG \
  --name $ACR \
  --sku Basic \
  --admin-enabled true

# 2. Importar la imagen de DVWA desde Docker Hub hacia tu propio ACR (una sola vez)
az acr import \
  --name $ACR \
  --source docker.io/vulnerables/web-dvwa:latest \
  --image dvwa:lab
```

> ⚠️ **Si el paso 2 también falla con `toomanyrequests`**: significa que el límite anónimo ya se agotó a nivel de todo el datacenter en ese momento. Dos alternativas, en orden de preferencia:
> 1. **Reintenta en 15-20 minutos** — el cupo anónimo de Docker Hub se restablece por ventanas de tiempo.
> 2. **Autentica la importación** con una cuenta gratuita de Docker Hub (el límite autenticado es más alto que el anónimo):
>    ```bash
>    az acr import \
>      --name $ACR \
>      --source docker.io/vulnerables/web-dvwa:latest \
>      --image dvwa:lab \
>      --username "<tu-usuario-dockerhub>" \
>      --password "<tu-password-o-access-token-dockerhub>"
>    ```

Verifica que la imagen ya vive en tu ACR (y no dependerás más de Docker Hub para el resto del laboratorio):

```bash
az acr repository show-tags --name $ACR --repository dvwa --output table
```

Obtén las credenciales que usará ACI para autenticarse contra tu ACR:

```bash
export ACR_LOGIN_SERVER=$(az acr show --name $ACR --query loginServer -o tsv)
export ACR_USERNAME=$(az acr credential show --name $ACR --query username -o tsv)
export ACR_PASSWORD=$(az acr credential show --name $ACR --query "passwords[0].value" -o tsv)
echo "Imagen lista en: $ACR_LOGIN_SERVER/dvwa:lab"
```

📸 **Captura para el informe — 3.1**: salida de `az acr repository show-tags` confirmando que el tag `lab` existe en tu propio registro.

---

## 3.2 Desplegar DVWA en Azure Container Instances, dentro de la VNet

Igual que con una VM, el contenedor se despliega con una **IP privada dentro de tu VNet-Spoke-App** — nunca expuesto directamente a Internet. La diferencia frente al paso equivalente de sesiones anteriores es que ahora el `--image` apunta a **tu propio ACR**, no a Docker Hub.

1. Crea una subred dedicada y delegada para ACI (ACI la exige exclusiva):

```bash
az network vnet subnet create \
  --resource-group $RG \
  --vnet-name $SPOKE_APP \
  --name $ACI_SUBNET \
  --address-prefixes 10.1.99.0/24 \
  --delegations Microsoft.ContainerInstance/containerGroups
```

2. Despliega el contenedor **desde tu propio ACR**:

```bash
az container create \
  --resource-group $RG \
  --name dvwa-lab \
  --image "$ACR_LOGIN_SERVER/dvwa:lab" \
  --registry-login-server "$ACR_LOGIN_SERVER" \
  --registry-username "$ACR_USERNAME" \
  --registry-password "$ACR_PASSWORD" \
  --vnet $SPOKE_APP \
  --subnet $ACI_SUBNET \
  --ports 80 \
  --cpu 1 \
  --memory 1
```

3. Obtén la IP privada asignada al contenedor (será el *backend* del Application Gateway):

```bash
export DVWA_IP=$(az container show --resource-group $RG --name dvwa-lab --query "ipAddress.ip" --output tsv)
echo "DVWA backend (IP privada): $DVWA_IP"
```

📸 **Captura para el informe — 3.2**: salida de `az container show` con la IP privada de DVWA, y confirmación visual de que el `--image` usado fue el de tu propio ACR (no `docker.io/...`).

---

## 3.3 Subred e IP pública del Application Gateway

Application Gateway exige una **subred dedicada**, vacía de otros recursos, con línea de vista al backend por el peering Hub↔Spoke de la Sesión 4.

```bash
az network vnet subnet create \
  --resource-group $RG --vnet-name $HUB --name $APPGW_SUBNET \
  --address-prefixes 10.0.99.0/24

az network public-ip create \
  --resource-group $RG --name "${APPGW}-pip" \
  --sku Standard --allocation-method Static
```

> 📝 Ajusta `10.0.99.0/24` si ese rango ya está en uso en tu VNet-Hub — revisa con `az network vnet subnet list --resource-group $RG --vnet-name $HUB --output table` antes de crear esta subred.

---

## 3.4 Crear la política de WAF (OWASP CRS) — empezamos en modo Detection

Usamos el **OWASP Core Rule Set 3.2** — incluye las reglas contra **SQL Injection** (grupo 942xxx) y **Command Injection** (grupo 932xxx). A propósito la dejamos primero en **Detection**: en este modo el WAF **registra** cada coincidencia con una regla, pero **no bloquea nada** — el ataque llega a DVWA exactamente igual que si el WAF no existiera. Esto es lo que vas a comprobar en el paso 3.10, antes de activar el bloqueo real.

```bash
# 1. Crear la política
az network application-gateway waf-policy create \
  --resource-group $RG --name $WAF_POLICY \
  --type OWASP --version 3.2

# 2. Dejarla en Detection (registra, no bloquea) — la comprobaremos así primero
az network application-gateway waf-policy policy-setting update \
  --policy-name $WAF_POLICY --resource-group $RG \
  --mode Detection --state Enabled

export WAF_POLICY_ID=$(az network application-gateway waf-policy show \
  --resource-group $RG --name $WAF_POLICY --query id --output tsv)
```

📸 **Captura para el informe — 3.3**: salida de `policy-setting update` (o el portal) mostrando `mode: Detection` y `state: Enabled`.

---

## 3.5 Crear el Application Gateway WAF_v2 con backend a DVWA

```bash
az network application-gateway create \
  --resource-group $RG --name $APPGW \
  --location $LOC \
  --sku WAF_v2 --capacity 1 \
  --vnet-name $HUB --subnet $APPGW_SUBNET \
  --public-ip-address "${APPGW}-pip" \
  --servers $DVWA_IP \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --waf-policy $WAF_POLICY_ID \
  --priority 100
```

> ⏱️ Este despliegue tarda entre **10 y 20 minutos** — es el paso más lento del laboratorio. Usa `--capacity 1` (fija, sin autoescalado): es más económica y más predecible para un laboratorio que el autoescalado.

Obtén la IP pública del Application Gateway:

```bash
export APPGW_PIP=$(az network public-ip show --resource-group $RG --name "${APPGW}-pip" --query ipAddress --output tsv)
echo "DVWA (por ahora con el WAF en Detection) en: http://$APPGW_PIP/"
```

---

## 3.6 NSG de la subred del Application Gateway

La subred del Application Gateway necesita permitir explícitamente el tráfico de **gestión del propio servicio** (obligatorio para WAF_v2) y el tráfico HTTP entrante desde Internet.

```bash
az network nsg create --resource-group $RG --name "nsg-$APPGW_SUBNET"

# Tráfico de gestión del App Gateway (obligatorio para WAF_v2)
az network nsg rule create --resource-group $RG --nsg-name "nsg-$APPGW_SUBNET" \
  --name Allow-GatewayManager --priority 100 --direction Inbound --access Allow \
  --protocol Tcp --source-address-prefixes GatewayManager \
  --source-port-range '*' --destination-address-prefix '*' --destination-port-range 65200-65535

# HTTP entrante desde Internet
az network nsg rule create --resource-group $RG --nsg-name "nsg-$APPGW_SUBNET" \
  --name Allow-HTTP-Internet --priority 110 --direction Inbound --access Allow \
  --protocol Tcp --source-address-prefix Internet \
  --source-port-range '*' --destination-address-prefix '*' --destination-port-range 80

# Asociar el NSG a la subred del App Gateway
az network vnet subnet update --resource-group $RG --vnet-name $HUB --name $APPGW_SUBNET \
  --network-security-group "nsg-$APPGW_SUBNET"
```

🧪 **Checkpoint 3.A**: sin la regla `Allow-GatewayManager`, el Application Gateway puede quedar en estado degradado o dejar de responder a los cambios de configuración. Si algo en esta sección se comporta de forma extraña, revisa primero este NSG.

---

## 3.7 Verificar la salud del backend

```bash
az network application-gateway show-backend-health \
  --resource-group $RG --name $APPGW \
  --query "backendAddressPools[].backendHttpSettingsCollection[].servers[].health" --output tsv
```

Debe devolver `Healthy`.

> ⚠️ **Si aparece `Unhealthy`**: el sondeo por defecto espera un código `200-399` en `/`. DVWA normalmente responde con una redirección (`302`) hacia `login.php`, lo cual **sí** se considera saludable — pero si tu instancia de DVWA usa una ruta distinta, crea un *probe* personalizado:
> ```bash
> az network application-gateway probe create \
>   --resource-group $RG --gateway-name $APPGW \
>   --name probe-dvwa --protocol Http --host-name-from-http-settings true \
>   --path /login.php --match-status-codes 200-399
> ```
> Si sigue `Unhealthy` después de esto, confirma que el NSG del spoke de aplicación permite tráfico desde `10.0.99.0/24` (la subred del App Gateway) hacia la subred de ACI en el puerto 80 — el mismo principio de microsegmentación de la Sesión 4.

📸 **Captura para el informe — 3.4**: salida de `show-backend-health` mostrando `Healthy`.

---

## 3.8 Habilitar Diagnostic Settings del Application Gateway (antes de probar los ataques)

Este paso es imprescindible **antes** de la Sección 3.10 — sin él, el WAF en modo Detection detecta el ataque, pero tú no tendrás forma de comprobarlo en los logs.

1. Ve al recurso `agw-dvwa-s5` en el portal.
2. **Diagnostic settings** (menú **Monitoring**) → **+ Add diagnostic setting**.
3. Nombre: `diag-agw-a-law`.
4. Marca `ApplicationGatewayAccessLog` y `ApplicationGatewayFirewallLog`.
5. Destino: **Send to Log Analytics workspace** → `law-seguridad-nube-s5` (creado en la Sección 2).
6. **Save**.

---

## 3.9 Preparar DVWA para las pruebas

Abre `http://$APPGW_PIP/` en el navegador. Deberías ver la pantalla de inicio de **DVWA** (usuario `admin` / contraseña `password` — solo para este laboratorio educativo). Inicia sesión y:

1. En el menú **Setup / Reset DB**, confirma que la base de datos está creada (o créala si es la primera vez).
2. En **DVWA Security**, fija el nivel en **low** — necesario para que los ataques de los pasos 3.10 y 3.11 se comporten como se espera.

📸 **Captura para el informe — 3.5**: pantalla de inicio de DVWA cargando a través de la IP pública del Application Gateway, con **DVWA Security = low** confirmado.

---

## 3.10 Fase 1 — Probar el ataque con el WAF en modo Detection

Con la política todavía en **Detection** (paso 3.4), ejecuta el mismo ataque que usarás después para comprobar el bloqueo.

**SQL Injection** (desde Cloud Shell):

```bash
curl -s -o /dev/null -w "%{http_code}\n" "http://$APPGW_PIP/vulnerabilities/sqli/?id=1' OR '1'='1&Submit=Submit"
```

**Resultado esperado en Detection**: código **`200`** — el ataque **llega hasta DVWA y se ejecuta con éxito**, exactamente igual que si el Application Gateway no tuviera WAF.

**Command Injection** (manualmente, desde el navegador, en el módulo *Command Injection* de DVWA, campo IP):

```
127.0.0.1; ls
```

**Resultado esperado en Detection**: el comando **se ejecuta** y ves la salida del `ls` en la página — el WAF no lo detuvo.

📸 **Captura para el informe — 3.6**: el código `200` del `curl`, y la captura del navegador mostrando la salida del `ls` (Command Injection exitoso).

Ahora confirma, en los logs, que el WAF **sí vio** el ataque aunque no lo bloqueó. En tu Log Analytics Workspace → **Logs**:

```kql
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| where TimeGenerated > ago(30m)
| project TimeGenerated, action_s, ruleId_s, Message, requestUri_s
| order by TimeGenerated desc
```

> 📝 Si los nombres de columna no coinciden exactamente (`action_s` vs `action`, etc.), ejecuta primero `... | take 5` sin el `project` y revisa qué columnas trajo tu workspace — el esquema puede variar ligeramente según la versión del diagnóstico.

Busca las filas correspondientes a tus dos ataques. El campo de acción debe mostrar algo equivalente a **`Detected`** (no `Blocked`) — la prueba, en logs, de que el modo Detection observa sin intervenir.

📸 **Captura para el informe — 3.7**: resultado de la consulta KQL mostrando tus dos ataques con acción `Detected`.

🧪 **Checkpoint 3.B**: si no ves ninguna fila después de 10-15 minutos, confirma que el paso 3.8 se guardó correctamente y que efectivamente generaste tráfico contra `$APPGW_PIP` (no directamente contra `$DVWA_IP`, que saltaría por completo al WAF).

---

## 3.11 Fase 2 — Cambiar a Prevention y repetir el mismo ataque

Ahora sí activamos el bloqueo real:

```bash
az network application-gateway waf-policy policy-setting update \
  --policy-name $WAF_POLICY --resource-group $RG \
  --mode Prevention --state Enabled
```

> ⏱️ El cambio de modo se propaga en menos de un minuto — no hace falta recrear ni reiniciar el Application Gateway.

Repite **exactamente los mismos ataques** del paso 3.10:

```bash
curl -s -o /dev/null -w "%{http_code}\n" "http://$APPGW_PIP/vulnerabilities/sqli/?id=1' OR '1'='1&Submit=Submit"
```

```
127.0.0.1; ls
```

**Resultado esperado en Prevention**: ambos deben devolver **`403 Forbidden`** — el Application Gateway bloquea la petición antes de que llegue a DVWA. La regla del grupo **942xxx** (SQLi) y **932xxx** (Command Injection/RCE) del OWASP CRS es la que actúa en cada caso.

📸 **Captura para el informe — 3.8**: el código `403` del `curl`, y la captura del navegador mostrando la página de bloqueo del WAF para el Command Injection.

Confirma también en los logs que ahora la acción cambió:

```kql
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| where TimeGenerated > ago(15m)
| project TimeGenerated, action_s, ruleId_s, Message, requestUri_s
| order by TimeGenerated desc
```

📸 **Captura para el informe — 3.9**: resultado de la consulta mostrando tus dos ataques ahora con acción `Blocked`.

🧪 **Checkpoint 3.C**: si el resultado sigue siendo `200` (o el comando sigue ejecutándose) en vez de `403`, revisa que el `policy-setting update` de este paso realmente se aplicó (`az network application-gateway waf-policy policy-setting show --policy-name $WAF_POLICY --resource-group $RG`) y que el Application Gateway sigue usando `$WAF_POLICY_ID` (paso 3.5).

---

## 3.12 Completar la tabla comparativa para el informe

Con lo observado en 3.10 y 3.11, completa esta tabla en tu informe:

| Ataque | Modo Detection (3.10) | Modo Prevention (3.11) |
|---|---|---|
| SQL Injection (`curl`) | Código HTTP: `___` — ¿se ejecutó? | Código HTTP: `___` — ¿se ejecutó? |
| Command Injection (`ls`) | ¿Se vio la salida del comando? | ¿Se vio la salida del comando? |
| Acción en `ApplicationGatewayFirewallLog` | `___` | `___` |

Esta tabla es, literalmente, tu evidencia de que **el WAF no es una casilla de configuración decorativa** — el mismo ataque, contra la misma aplicación, tiene un resultado radicalmente distinto según el modo de la política.

---

## Solución de problemas frecuentes de esta sección

| Síntoma | Causa probable | Solución |
|---|---|---|
| `az acr import` o el despliegue de ACI falla con `toomanyrequests` | Límite de *pulls* anónimos de Docker Hub agotado en el rango de IP de salida de Azure | Reintenta en 15-20 min, o autentica la importación con una cuenta gratuita de Docker Hub (`--username`/`--password` en `az acr import`) |
| Backend *Unhealthy* en el Application Gateway | Sondeo por defecto, o NSG entre subredes | Probe personalizado a `/login.php` (paso 3.7); verificar NSG entre `AppGatewaySubnet` y `ACISubnet` |
| En el paso 3.10 el ataque no se ejecuta (ya llega bloqueado) | La política quedó en `Prevention` desde el paso 3.4 en vez de `Detection` | Revisar `policy-setting update` del paso 3.4; confirmar con `policy-setting show` antes de continuar |
| En el paso 3.11 el ataque NO se bloquea | El cambio a `Prevention` no se aplicó, o el gateway no usa `$WAF_POLICY_ID` | Confirmar `policy-setting show` y que `--waf-policy` se usó al crear el gateway (paso 3.5) |
| No aparecen filas en `ApplicationGatewayFirewallLog` | Diagnostic settings del paso 3.8 no configurado, o insuficiente tiempo de propagación | Revisar el paso 3.8; esperar 10-15 min adicionales tras generar el tráfico |
| `az acr create` falla con "the registry name is not available" | El nombre de ACR debe ser único **a nivel global** en Azure | Cambia `$ACR` por algo más específico, por ejemplo agregando tus iniciales o un número |

---

## 🧩 Preguntas de repaso — Sección 3

1. En tus propias palabras: ¿qué diferencia de comportamiento observaste entre el modo `Detection` y el modo `Prevention` frente al mismo ataque de SQL Injection?
2. ¿Por qué es pedagógicamente más convincente probar el mismo ataque dos veces (Detection y luego Prevention) que solo desplegar directamente en Prevention y confiar en que "funciona"?
3. En un entorno productivo real, ¿en qué situación tendría sentido dejar una política de WAF en modo `Detection` de forma prolongada, en vez de pasar a `Prevention` de inmediato?
4. Explica para qué sirve la regla NSG `Allow-GatewayManager` del paso 3.6, y qué le pasaría al Application Gateway si esa regla no existiera.

---

[← Anterior: 2. Habilitar visibilidad](02-Habilitar-Visibilidad.md) · [← Volver al README](README.md) · Siguiente: [4. Simulación del incidente →](04-Simulacion-Incidente.md)
