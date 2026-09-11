[← Volver al README](README.md) · Sección 3 de 7 · Parte B — Detección

# 3. Despliegue del WAF y DVWA (el componente diferido de la Sesión 4)

## Objetivo de la sección

Desplegar el componente que quedó pendiente al cerrar la Sesión 4: una aplicación deliberadamente vulnerable (**DVWA — Damn Vulnerable Web Application**) protegida por un **Application Gateway con Web Application Firewall (WAF_v2)**. Este será el "activo público" que, más adelante, sirve de contexto para el escenario de incidente.

> 💰 **Aviso de costo**: Application Gateway y ACI **no se pueden apagar** como una VM — cobran por hora mientras existen. La Sección 7 termina con su eliminación explícita. No dejes estos recursos corriendo entre sesiones de trabajo.

---

## 3.1 Crear una subred dedicada para Application Gateway

Application Gateway necesita su propia subred, vacía de otros recursos.

```bash
az network vnet subnet create \
  --resource-group <RG> \
  --vnet-name <nombre-vnet-hub> \
  --name AppGatewaySubnet \
  --address-prefixes 10.0.99.0/24
```

> 📝 Ajusta el prefijo `10.0.99.0/24` si ese rango ya está en uso en tu VNet — revisa tus subredes existentes con `az network vnet subnet list --resource-group <RG> --vnet-name <nombre-vnet-hub> --output table` antes de crear esta.

---

## 3.2 Desplegar DVWA en Azure Container Instances, dentro de la VNet

A diferencia de una VM, un contenedor ACI se despliega directamente con una **IP privada dentro de tu VNet-Spoke-App** — sin exponerlo nunca directamente a Internet.

1. Primero, crea una subred dedicada para ACI dentro de tu VNet-Spoke-App (ACI requiere una subred exclusiva, delegada):

```bash
az network vnet subnet create \
  --resource-group <RG> \
  --vnet-name <nombre-vnet-spoke-app> \
  --name ACISubnet \
  --address-prefixes 10.1.99.0/24 \
  --delegations Microsoft.ContainerInstance/containerGroups
```

2. Despliega el contenedor DVWA:

```bash
az container create \
  --resource-group <RG> \
  --name dvwa-lab \
  --image vulnerables/web-dvwa \
  --vnet <nombre-vnet-spoke-app> \
  --subnet ACISubnet \
  --ports 80 \
  --cpu 1 \
  --memory 1
```

3. Obtén la IP privada asignada al contenedor:

```bash
az container show \
  --resource-group <RG> \
  --name dvwa-lab \
  --query "ipAddress.ip" \
  --output tsv
```

Anota esta IP — la necesitas en el paso 3.4.

📸 **Captura para el informe — 3.1**: salida de `az container show` con la IP privada de DVWA.

---

## 3.3 Crear el Application Gateway con WAF_v2

Este comando crea el Application Gateway, su IP pública y el listener básico en un solo paso. Sustituye `<IP-DVWA>` por la IP obtenida en el paso anterior.

```bash
az network public-ip create \
  --resource-group <RG> \
  --name pip-agw-s5 \
  --sku Standard \
  --allocation-method Static

az network application-gateway create \
  --resource-group <RG> \
  --name agw-dvwa-s5 \
  --location <tu-region> \
  --vnet-name <nombre-vnet-hub> \
  --subnet AppGatewaySubnet \
  --public-ip-address pip-agw-s5 \
  --sku WAF_v2 \
  --min-capacity 0 \
  --max-capacity 2 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --frontend-port 80 \
  --servers <IP-DVWA>
```

> ⏱️ Este despliegue puede tardar entre **10 y 20 minutos**. Es el paso más lento de todo el laboratorio — aprovecha para leer la Sección 4 mientras esperas.

> 💡 `--min-capacity 0` permite que Application Gateway escale a cero unidades cuando no hay tráfico, reduciendo el costo — pero **sigue cobrando un cargo fijo por hora mientras el recurso exista**, escale o no.

---

## 3.4 Activar el modo Prevention del WAF

Por defecto, `WAF_v2` se crea en modo **Detection** (registra pero no bloquea). Para este laboratorio lo queremos en **Prevention**:

```bash
az network application-gateway waf-config set \
  --resource-group <RG> \
  --gateway-name agw-dvwa-s5 \
  --enabled true \
  --firewall-mode Prevention \
  --rule-set-type OWASP \
  --rule-set-version 3.2
```

📸 **Captura para el informe — 3.2**: configuración del WAF mostrando `firewall-mode: Prevention` y el rule set OWASP 3.2.

---

## 3.5 Habilitar Diagnostic Settings del Application Gateway

Igual que hicimos en la Sección 2 con otros recursos:

1. Ve al recurso **agw-dvwa-s5** en el portal.
2. **Diagnostic settings** (menú **Monitoring**) → **+ Add diagnostic setting**.
3. Nombre: `diag-agw-a-law`.
4. Marca `ApplicationGatewayAccessLog` y `ApplicationGatewayFirewallLog`.
5. Destino: **Send to Log Analytics workspace** → `law-seguridad-nube-s5`.
6. **Save**.

---

## 3.6 Verificar acceso a DVWA a través del Application Gateway

1. Obtén la IP pública del Application Gateway:

```bash
az network public-ip show \
  --resource-group <RG> \
  --name pip-agw-s5 \
  --query "ipAddress" \
  --output tsv
```

2. Abre esa IP en el navegador: `http://<IP-PUBLICA-AGW>`.
3. Deberías ver la pantalla de inicio de **DVWA** (usuario por defecto: `admin` / contraseña: `password` — solo para efectos de este laboratorio educativo; nunca uses estas credenciales en un entorno real).

📸 **Captura para el informe — 3.3**: pantalla de login de DVWA cargando a través de la IP pública del Application Gateway.

---

## 3.7 Confirmar que el WAF bloquea un intento de ataque básico

Desde Cloud Shell, simula un intento de SQL Injection simple contra DVWA a través del Application Gateway:

```bash
curl -s -o /dev/null -w "%{http_code}\n" "http://<IP-PUBLICA-AGW>/vulnerabilities/sqli/?id=1' OR '1'='1&Submit=Submit"
```

Si el WAF está funcionando en modo Prevention, deberías recibir un código **403 Forbidden** en lugar de un `200`.

🧪 **Checkpoint 3.A**: si el resultado es `200` en vez de `403`, revisa el paso 3.4 — el firewall-mode probablemente sigue en `Detection` o no se guardó correctamente.

📸 **Captura para el informe — 3.4**: salida del comando `curl` mostrando el código `403`.

---

## 🧩 Preguntas de repaso — Sección 3

1. ¿Por qué DVWA se despliega con una IP **privada** dentro de la VNet en lugar de exponerlo directamente con una IP pública?
2. ¿Qué diferencia práctica hay entre el modo `Detection` y el modo `Prevention` de un WAF, y por qué el laboratorio pide explícitamente `Prevention`?
3. Explica con tus palabras qué hace el ACISubnet delegado (`Microsoft.ContainerInstance/containerGroups`) y por qué ACI lo necesita.
4. Si el `curl` del paso 3.7 hubiera devuelto `502 Bad Gateway` en lugar de `403`, ¿qué componente sospecharías que está mal configurado: el WAF, el backend pool, o la conectividad de red? Justifica tu respuesta.

---

[← Anterior: 2. Habilitar visibilidad](02-Habilitar-Visibilidad.md) · [← Volver al README](README.md) · Siguiente: [4. Simulación del incidente →](04-Simulacion-Incidente.md)
