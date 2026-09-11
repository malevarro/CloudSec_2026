[← Volver al README](README.md) · Sección 1 de 7 · Parte A — Visibilidad

# 1. Preparación y reactivación de la arquitectura de la Sesión 4

## Objetivo de la sección

Confirmar que la arquitectura de la Sesión 4 sigue intacta, reactivar las máquinas virtuales que quedaron *deallocadas* (apagadas y sin cobrar cómputo) al cierre de esa sesión, y dejar el entorno listo para construir sobre él.

No vamos a modificar nada de la Sesión 4 en esta sección — solo verificar y encender.

---

## 1.1 Iniciar sesión en Azure

1. Abre tu navegador y ve a **https://portal.azure.com**.
2. Inicia sesión con la misma cuenta que usaste en las Sesiones 1 a 4.
3. En la barra superior, haz clic en el ícono **`>_`** (Cloud Shell), a la derecha de la barra de búsqueda.
4. Si es la primera vez que abres Cloud Shell en esta sesión de navegador, Azure te preguntará **Bash o PowerShell**. Elige **Bash**.
5. Espera a que aparezca el prompt (`usuario@Azure:~$`). Esto puede tardar 30-60 segundos la primera vez.

📸 **Captura para el informe — 1.1**: pantalla del portal con Cloud Shell abierto y el prompt de Bash visible.

---

## 1.2 Confirmar la suscripción activa

En Cloud Shell, ejecuta:

```bash
az account show --output table
```

Deberías ver el nombre de tu suscripción (por ejemplo, "Azure subscription 1" o "Free Trial") y el `IsDefault` en `True`.

Si tienes más de una suscripción y no es la correcta, cámbiala con:

```bash
az account set --subscription "<nombre-o-ID-de-tu-suscripción>"
```

---

## 1.3 Listar los resource groups existentes

```bash
az group list --output table
```

Identifica el **resource group de la Sesión 4** (el nombre que usaste entonces, por ejemplo `rg-seguridad-nube-s4` o el que hayas definido). Anótalo — lo usaremos en todos los comandos siguientes de esta guía.

> 📝 A partir de aquí, cada vez que veas `<RG>` en un comando, reemplázalo por el nombre real de tu resource group.

---

## 1.4 Verificar que los componentes de la Sesión 4 siguen presentes

Lista todos los recursos del grupo:

```bash
az resource list --resource-group <RG> --output table
```

Verifica visualmente que aparecen (los nombres exactos dependen de cómo los llamaste en la Sesión 4):

- [ ] Una **VNet-Hub** con su subred de firewall/NVA.
- [ ] Dos **VNet-Spoke** (App y Datos).
- [ ] El **Firewall / NVA** (Zentyal Server u otro).
- [ ] **Azure Bastion**.
- [ ] Un **Key Vault**.
- [ ] Una **Storage Account**.
- [ ] Cuatro **máquinas virtuales**.

🧪 **Checkpoint 1.A**: si falta alguno de estos componentes, **detente aquí** y contacta al instructor antes de continuar. El resto del laboratorio depende de que esta base exista completa.

📸 **Captura para el informe — 1.2**: salida completa de `az resource list` mostrando todos los recursos del grupo.

---

## 1.5 Verificar el estado actual de las VM

```bash
az vm list --resource-group <RG> --show-details --query "[].{Nombre:name, Estado:powerState}" --output table
```

Todas deberían mostrar `VM deallocated` (así quedaron al cierre de la Sesión 4).

---

## 1.6 Reactivar las cuatro máquinas virtuales

Reactiva cada una por su nombre. Sustituye `<nombre-vm>` por el nombre real de cada VM (revisa la salida del paso 1.5):

```bash
az vm start --resource-group <RG> --name <nombre-vm-1>
az vm start --resource-group <RG> --name <nombre-vm-2>
az vm start --resource-group <RG> --name <nombre-vm-3>
az vm start --resource-group <RG> --name <nombre-vm-4>
```

> ⏱️ Cada comando puede tardar entre 1 y 3 minutos. Azure no te devuelve el control hasta que la VM termina de arrancar — es normal ver el cursor "colgado" un momento.

Si prefieres reactivarlas todas en una sola línea:

```bash
for vm in <nombre-vm-1> <nombre-vm-2> <nombre-vm-3> <nombre-vm-4>; do
  az vm start --resource-group <RG> --name "$vm" --no-wait
done
```

El flag `--no-wait` lanza las cuatro reactivaciones en paralelo sin esperar. Verifica el progreso con el comando del paso siguiente.

---

## 1.7 Confirmar que todas quedaron encendidas

```bash
az vm list --resource-group <RG> --show-details --query "[].{Nombre:name, Estado:powerState}" --output table
```

Espera hasta que las cuatro muestren `VM running`.

📸 **Captura para el informe — 1.3**: tabla con las 4 VM en estado `VM running`.

🧪 **Checkpoint 1.B**: confirma que puedes conectarte a al menos una VM vía Azure Bastion (Portal → tu VM → **Connect** → **Bastion**), tal como lo hiciste en la Sesión 4. No necesitas hacer nada dentro de la VM — solo confirmar que la conexión administrativa sigue funcionando.

📸 **Captura para el informe — 1.4**: sesión de Bastion abierta contra una de las VM.

---

## 🧩 Preguntas de repaso — Sección 1

1. ¿Por qué el cierre de la Sesión 4 usó `deallocate` en lugar de `delete` para las máquinas virtuales? ¿Qué diferencia de costo implica cada opción?
2. ¿Qué comando de Azure CLI usarías para reactivar una VM, y qué comando usarías para volver a dejarla en estado `deallocated`?
3. Si al ejecutar `az resource list` faltara el Key Vault de la Sesión 3/4, ¿qué componentes de las secciones siguientes de este laboratorio dejarían de funcionar, y por qué?
4. ¿Por qué seguimos usando Azure Bastion para conectarnos a las VM en lugar de asignarles una IP pública, incluso en un laboratorio de detección de incidentes?

---

[← Volver al README](README.md) · Siguiente: [2. Habilitar visibilidad →](02-Habilitar-Visibilidad.md)
