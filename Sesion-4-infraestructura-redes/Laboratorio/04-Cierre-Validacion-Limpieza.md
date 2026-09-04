[← Parte B — Acceso Privado a PaaS](03-Parte-B-Acceso-Privado-PaaS.md) | [Rúbrica de evaluación →](RUBRICA-EVALUACION.md)

# 4. Cierre, Validación y Puesta en Pausa

> ⚠️ **Este laboratorio ya no termina eliminando los recursos.** La arquitectura que construiste en las Partes A y B es exactamente la que vas a reutilizar en el laboratorio de la **Sesión 5**, donde se agrega el Application Gateway con WAF y las capacidades de observabilidad/respuesta a incidentes. Por eso, en lugar de borrar el grupo de recursos, esta sección te guía para dejarlo en **estado de mínimo consumo**: la red, las identidades y los datos permanecen; el cómputo que factura por hora se apaga.

## 4.1 Checklist de finalización

**Parte A — Arquitectura Hub-Spoke con Zentyal**
- [ ] 3 VNets creadas, cada spoke con su propia `WorkloadSubnet`: `Hub-VNet` (10.0.0.0/16), `Spoke-App-VNet` (10.1.0.0/16), `Spoke-Data-VNet` (10.2.0.0/16).
- [ ] `FW-Zentyal` corriendo en `Standard_B2s`, dual-NIC, con IP forwarding habilitado en Azure.
- [ ] Zentyal Server instalado y accesible en `https://<IP>:8443`; interfaces `eth0 = External` / `eth1 = Internal` confirmadas.
- [ ] Módulos Firewall, Gateway (NAT) e IDS/IPS activos en Zentyal.
- [ ] Reglas de firewall internas y externas configuradas en el orden correcto, con `Deny-All` al final de cada tabla.
- [ ] IDS/IPS (Suricata) con rulesets `emerging-scan`, `emerging-policy`, `emerging-shellcode` habilitados.
- [ ] SSH/8443 de Zentyal y SSH de las tres VMs de spoke/app restringido a `MI_IP`.
- [ ] VNet Peering Hub↔Spoke-App y Hub↔Spoke-Data, con `forwarded traffic` habilitado.
- [ ] `RT-Spoke-App` y `RT-Spoke-Data` asociadas correctamente, con next hop `VirtualAppliance` hacia Zentyal.
- [ ] `VM-Spoke-App` y `VM-Spoke-Data` desplegadas, con `nmap`/`netcat` instalados.
- [ ] Prueba de escaneo `nmap` entre spokes detectada en `/var/log/suricata/fast.log`.
- [ ] Prueba de egreso (`curl ifconfig.me` desde `VM-Spoke-App`) devolviendo la IP pública de Zentyal.
- [ ] `NSG-AppIntegration` con deny-by-default y solo 2 excepciones documentadas.
- [ ] `AppGatewaySubnet` creada (vacía, reservada para la Sesión 5).

**Parte B — Acceso Privado a PaaS**
- [ ] Decisión documentada: Opción 1 (Azure SQL) u Opción 2 (Storage Account), con evidencia si hubo cambio de opción por error de cuota.
- [ ] Recurso de datos y Key Vault con `publicNetworkAccess: Disabled`.
- [ ] Private Endpoints de datos y Key Vault en `provisioningState: Succeeded`, con zonas DNS privadas enlazadas.
- [ ] `VM-App` desplegada con identidad administrada del sistema, dentro de `AppIntegrationSubnet`.
- [ ] `AppIntegrationSubnet` asociada a `RT-Spoke-App` (tráfico de `VM-App` forzado por Zentyal).
- [ ] Identidad de `VM-App` con el rol correspondiente (Key Vault Secrets User + `db_datareader/db_datawriter` o Storage Blob Data Contributor, según tu opción).
- [ ] Regla `Allow-App-to-Datos` creada en el panel de Zentyal, ubicada antes de `Deny-All`.
- [ ] `az login --identity` y `nslookup` ejecutados desde `VM-App`, resolviendo a IPs de `10.2.1.0/24`.

## 4.2 Solución de problemas frecuentes

| Síntoma | Causa probable | Solución |
|---|---|---|
| No puedo conectarme por SSH ni al panel 8443 de Zentyal | Tu IP pública cambió | `curl ifconfig.me` y actualiza las reglas NSG con `az network nsg rule update` |
| Interfaces no usan `eth0`/`eth1` | Ubuntu 24.04 usa nombres predictivos | Aplica el ajuste de GRUB de la sección 2.6.1 y reinicia |
| El instalador de Zentyal aborta | Paquetes pendientes, o error de descarga de la clave GPG | `sudo apt dist-upgrade -y` y repite; confirma que el script use el `wget` con `-U` (user agent) indicado |
| El asistente inicial de Zentyal no avanza | Se seleccionaron módulos incompletos o se reinició antes de configurar Network | Repite el asistente desde Module Status, sin reiniciar hasta configurar interfaces |
| No hay conectividad Spoke-to-Spoke | Peering sin *forwarded traffic*, UDR ausente, o regla de Zentyal mal ubicada | Revisa Pasos 12, 13 y las tablas de reglas de la sección 2.10 |
| `nmap` no genera alertas en `fast.log` | Suricata no está monitoreando la interfaz correcta, o el ruleset no está habilitado | Revisa sección 2.11.2 (interfaces) y 2.11.3 (rulesets); confirma `systemctl status suricata` |
| `curl ifconfig.me` desde una VM de spoke no devuelve la IP de Zentyal | Falta la ruta UDR en esa subred, o el NAT/masquerade no está configurado | Revisa Paso 13 y sección 2.10.2 |
| Azure SQL falla al crear (cuota, `SubscriptionNotRegistered`, SKU no disponible) | Limitación conocida y frecuente en cuentas Free Tier/de prueba | Cambia a la **Opción 2 (Storage Account)**, sección 3.4 |
| `az storage account create` falla por nombre duplicado | El nombre no es único globalmente | Cambia `MI_SUFIJO` por algo más específico (agrega un número) |
| La app no resuelve el nombre de datos/Key Vault a IP privada | Zona DNS privada no enlazada, o falta el `dns-zone-group` del Private Endpoint | Revisa Paso 15 y 18a/18b |
| `Login failed for user '<token-identified principal>'` (Opción 1) | Usuario `FROM EXTERNAL PROVIDER` no creado, o su nombre no coincide exactamente con `VM-App` | Repite el T-SQL del Paso 23, verificando el nombre exacto |
| `VM-App` no puede escribir un blob (Opción 2) | Falta el rol `Storage Blob Data Contributor` sobre la identidad de `VM-App` | Repite la asignación de rol del Paso 23 |
| `az login --identity` falla dentro de `VM-App` | La identidad administrada no se asignó, o Azure CLI no está instalado | Repite `az vm identity assign` (Paso 21) y confirma la instalación de Azure CLI |
| El tráfico App→Datos sigue bloqueado tras el Paso 24 | La regla `Allow-App-to-Datos` quedó después de `Deny-All` en Zentyal | Reordena la regla en el panel web, antes de `Deny-All` |
| Al reactivar para la Sesión 5, las VMs no arrancan o pierden su IP pública | Las IP públicas `Standard` reservadas conservan su dirección al reiniciar, pero si eliminaste alguna manualmente se genera una nueva | Verifica con `az network public-ip list` antes de continuar en la Sesión 5 y actualiza tus reglas NSG si la IP cambió |

## 4.3 Poner la arquitectura en pausa (mínimo consumo, sin eliminar nada)

En lugar de `az group delete`, vas a **detener (deallocate)** las cuatro máquinas virtuales. Un VM "deallocated" deja de facturar cómputo — que es, con diferencia, el costo más alto de esta arquitectura — mientras conserva su disco, su configuración de red, su IP asignada y todo lo que instalaste (Zentyal, `nmap`, la identidad administrada de `VM-App`, etc.). Redes, Private Endpoints, Key Vault y el recurso de datos (SQL o Storage) **se dejan corriendo**: su costo combinado es mínimo y son exactamente lo que la Sesión 5 necesita encontrar ya configurado.

```bash
az vm deallocate --resource-group RG-Sesion4-Redes --name FW-Zentyal
az vm deallocate --resource-group RG-Sesion4-Redes --name VM-Spoke-App
az vm deallocate --resource-group RG-Sesion4-Redes --name VM-Spoke-Data
az vm deallocate --resource-group RG-Sesion4-Redes --name VM-App
```

🧪 **Checkpoint:**
```bash
az vm list --resource-group RG-Sesion4-Redes -d --query "[].{Nombre:name, Estado:powerState}" --output table
```
Las cuatro VMs deben mostrar `VM deallocated`.

📸 **Captura 5.1 (última del laboratorio):** el resultado del comando anterior mostrando las cuatro VMs en estado `VM deallocated`.

> ℹ️ **Qué sigue costando algo, y por qué eso está bien:**
> - Al reemplazar App Service por `VM-App`, **todo** el cómputo de esta arquitectura son máquinas virtuales — y las cuatro se pueden desasignar por completo. Ya no queda ningún componente (como antes el plan de App Service) que siga facturando por hora sin poder apagarse.
> - **Key Vault**, el **recurso de datos** (SQL Basic ≈ USD 5/mes, o Storage Account, prácticamente centavos) y los **Private Endpoints** tienen un costo marginal y se dejan activos porque son exactamente el estado que la Sesión 5 necesita.
> - Las **IP públicas Standard** (Zentyal y las tres VMs restantes) tienen un pequeño costo por hora incluso con la VM apagada (reservan la dirección). Es intencional: así conservas las mismas IPs y no tienes que volver a ajustar ninguna regla de NSG al reactivar.

## 4.4 Cómo reactivar la arquitectura al iniciar la Sesión 5

Cuando comiences el laboratorio de la Sesión 5, lo primero será encender de nuevo las cuatro VMs:

```bash
az vm start --resource-group RG-Sesion4-Redes --name FW-Zentyal
az vm start --resource-group RG-Sesion4-Redes --name VM-Spoke-App
az vm start --resource-group RG-Sesion4-Redes --name VM-Spoke-Data
az vm start --resource-group RG-Sesion4-Redes --name VM-App
```

🧪 **Checkpoint:** repite el comando de la sección 4.3 — las cuatro VMs deben mostrar ahora `VM running`, y deberías poder conectarte por SSH con las mismas IPs de antes (confirma primero que tu `MI_IP` no haya cambiado).

> ⚠️ Si en algún momento decides que **no vas a continuar** con la Sesión 5 (por ejemplo, al final del curso), entonces sí elimina todo el grupo de recursos: `az group delete --name RG-Sesion4-Redes --yes --no-wait`. Esa instrucción ya no es el cierre por defecto de este laboratorio — solo se usa cuando la arquitectura deja de ser necesaria.

---

## 🧩 Preguntas de repaso generales (integran las Partes A y B)

1. Explica la diferencia entre **eliminar** un recurso y **desasignarlo (deallocate)**. ¿Por qué esta distinción existe para máquinas virtuales y no, por ejemplo, para Key Vault o para un Storage Account?
2. De los controles construidos en este laboratorio (Zentyal, Private Endpoints), ¿cuál combate mejor un ataque de **movimiento lateral** dentro de la red, y cuál combate mejor la **exfiltración de datos** hacia Internet?
3. Zentyal añadió una capacidad que un firewall hecho completamente a mano con `iptables` no habría dado "gratis": el IDS/IPS con Suricata. Explica, con tus propias palabras, qué tipo de amenaza detecta un IDS que un firewall de solo filtrado de paquetes no vería.
4. La sección 4.3 deja varios recursos corriendo deliberadamente (Key Vault, el recurso de datos, las IP públicas) en lugar de eliminarlos. Para cada uno, explica en una frase por qué su costo mientras está inactivo se considera aceptable frente al beneficio de no reconstruirlo en la Sesión 5.

---

## Cómo entregar

Consolida **todas** tus capturas (📸) y las respuestas a **todas** las preguntas de repaso (🧩) de esta guía en **un único documento PDF**, y súbelo a la tarea de la Sesión 4 en **Google Classroom**. Revisa la [Rúbrica de Evaluación](RUBRICA-EVALUACION.md) antes de entregar.

---

[← Parte B — Acceso Privado a PaaS](03-Parte-B-Acceso-Privado-PaaS.md) | [Rúbrica de evaluación →](RUBRICA-EVALUACION.md) | [Volver al README](README.md)
