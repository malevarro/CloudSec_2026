[⬅ Volver al índice](../README.md)

# Sección 5 — Mapeo de responsabilidades por servicio (VM vs. App Service vs. SaaS)

**Tiempo estimado:** 15–20 minutos

## 🎯 Objetivo de esta sección

Comparar, **dentro del propio Portal de Azure**, cuánta configuración de seguridad queda a tu cargo según elijas IaaS (Máquina Virtual), PaaS (App Service) o SaaS — completando tú mismo el diagrama de responsabilidad compartida con base en lo que observes.

> 💡 En esta sección **exploraremos** los formularios de creación de una VM y de un App Service **sin completarlos** (los cancelaremos antes del último paso) — el objetivo es ver qué le exige Azure configurar al cliente en cada modelo, no desplegar la VM todavía. El App Service real lo crearemos en la Sección 6.

---

## Paso 1 — Repaso rápido: el modelo de responsabilidad compartida

Recordarás de la teoría que la responsabilidad se reparte así:

| Capa | IaaS (VM) | PaaS (App Service) | SaaS (ej. Microsoft 365) |
|---|---|---|---|
| Datos e identidades | Cliente | Cliente | Cliente |
| Aplicación | Cliente | Cliente | Proveedor |
| Sistema operativo | Cliente | Proveedor | Proveedor |
| Hosts / red / datacenter | Proveedor | Proveedor | Proveedor |

Vamos a **verificarlo empíricamente**: observando qué te pide configurar cada formulario de creación, confirmamos en la práctica qué es responsabilidad tuya.

---

## Paso 2 — Explorar la creación de una Máquina Virtual (IaaS)

### ✅ 2.1

1. En el buscador global, escribe `Virtual machines` y ábrelo.
2. Haz clic en **+ Create → Azure virtual machine**.
3. **NO completes el formulario todavía** — solo recorre cada pestaña observando qué te pide.

### ✅ 2.2 Observa la pestaña "Basics"

Fíjate en particular en estos campos — anótalos, ya que los usarás para completar la tabla del Paso 4:

- **Image** (Imagen del sistema operativo): tú eliges qué SO instalar (Ubuntu, Windows Server, etc.) — y serás tú quien deba parchearlo.
- **Size** (Tamaño): tú eliges la capacidad de cómputo (CPU/RAM). Para mantenerte en el nivel gratuito, Azure ofrece 750 horas/mes de una VM tipo `B1s` durante los primeros 12 meses — identifícala en la lista si haces clic en **See all sizes**, pero **no la selecciones todavía**.
- **Administrator account:** tú defines el usuario y la contraseña o llave SSH de acceso al sistema operativo — **es tu responsabilidad protegerla**, Azure no la gestiona por ti.
- **Inbound port rules:** tú decides qué puertos de red están abiertos hacia la VM.

### ✅ 2.3 Observa la pestaña "Disks"

- Tú eliges el tipo de disco y **tú eres responsable de la configuración de cifrado**, aunque Azure ofrece cifrado en reposo por defecto.

### ✅ 2.4 Observa la pestaña "Networking"

- Tú configuras la red virtual, la subred y el Network Security Group (NSG) — es decir, el firewall a nivel de red de tu VM.

### ✅ 2.5 Cancelar sin crear

1. Sin hacer clic en "Review + create", simplemente **cierra la pestaña o navega hacia otra sección** del portal (por ejemplo, haciendo clic en "Home").
2. Confirma que no se creó ninguna VM revisando tu Resource Group `rg-lab1-<inic>` — debe seguir vacío.

> ⚠️ Si accidentalmente llegas a crear la VM, no te preocupes por el costo (está dentro del nivel gratuito), pero **elimínala de inmediato** desde el Resource Group para mantener el laboratorio limpio, ya que no la necesitamos.

---

## Paso 3 — Explorar la creación de un App Service (PaaS)

### ✅ 3.1

1. En el buscador global, escribe `App Services` y ábrelo.
2. Haz clic en **+ Create → Web App**.
3. De nuevo, **recorre el formulario sin crear todavía** (crearemos el definitivo en la Sección 6, con los valores exactos).

### ✅ 3.2 Observa la pestaña "Basics"

- **Publish:** puedes elegir `Code`, `Container` o `Static Web App` — tú decides cómo despliegas tu aplicación, pero **no configuras el sistema operativo subyacente**.
- **Runtime stack:** eliges el lenguaje/versión (Node.js, Python, .NET, etc.) — Azure se encarga de mantener el sistema operativo y el runtime parcheados.
- **Operating System:** puedes elegir Linux o Windows, pero **no accedes ni parcheas el sistema operativo tú mismo** — eso ya es tarea del proveedor en este modelo.
- **Pricing plan:** aquí es donde seleccionarás **F1 Free** en la Sección 6 — nota que ni siquiera existe la opción de elegir "tamaño de disco" o "imagen de SO" como en la VM: esas decisiones ya no son tuyas.

### ✅ 3.3 Cancelar sin crear

1. Navega fuera del formulario sin hacer clic en "Review + create".
2. Verifica que tu Resource Group sigue vacío.

### 🧪 Checkpoint

Notaste que el formulario de App Service **tiene muchos menos campos relacionados con infraestructura** que el de la Máquina Virtual — esa diferencia **es** la responsabilidad compartida en acción.

---

## Paso 4 — SaaS: comparación conceptual

A diferencia de IaaS y PaaS, un SaaS como **Microsoft 365** o **Salesforce** no se "crea" dentro de tu suscripción de Azure — es un producto completamente distinto, gestionado por su propio proveedor. No hay un formulario de creación de infraestructura que explorar, y esa es precisamente la lección: **como cliente, tu única superficie de control son los ajustes de la aplicación** (usuarios, permisos, políticas de retención, configuración de seguridad expuesta en el propio SaaS).

Si tienes acceso a una cuenta de Microsoft 365 (personal, institucional o de prueba), puedes opcionalmente:

1. Ir a **https://admin.microsoft.com** (si tienes permisos de administrador) o a tu panel de usuario.
2. Observar que las opciones disponibles son de **gestión de usuarios, licencias y políticas** — nunca de sistema operativo, red o parches.

Si no tienes acceso, no es un impedimento: la tabla del Paso 5 se completa igual con base en el concepto.

---

## Paso 5 — Completa tu tabla de responsabilidades

Con base en lo que observaste en los Pasos 2 y 3 (y el concepto del Paso 4), completa esta tabla en tu propia copia de la guía o en un archivo aparte. Escribe `Cliente`, `Proveedor` o `Compartida` en cada celda:

| Capa | VM (IaaS) | App Service (PaaS) | SaaS |
|---|---|---|---|
| Hardware físico / datacenter | | | |
| Hipervisor / virtualización | | | |
| Sistema operativo (parches) | | | |
| Runtime / middleware | | | |
| Código de la aplicación | | | |
| Configuración de red (NSG, puertos) | | | |
| Identidades y control de acceso | | | |
| Datos | | | |

<details>
<summary>💡 Ver tabla de respuesta (haz clic para expandir, verifica tu trabajo después de completarla tú mismo)</summary>

| Capa | VM (IaaS) | App Service (PaaS) | SaaS |
|---|---|---|---|
| Hardware físico / datacenter | Proveedor | Proveedor | Proveedor |
| Hipervisor / virtualización | Proveedor | Proveedor | Proveedor |
| Sistema operativo (parches) | Cliente | Proveedor | Proveedor |
| Runtime / middleware | Cliente | Proveedor | Proveedor |
| Código de la aplicación | Cliente | Cliente | Proveedor |
| Configuración de red (NSG, puertos) | Cliente | Compartida | Proveedor |
| Identidades y control de acceso | Cliente | Cliente | Cliente |
| Datos | Cliente | Cliente | Cliente |

</details>

---

## Paso 6 — Conecta esto con tu modelo de amenazas

Vuelve al modelo de amenazas que creaste en la [Sección 1](../01-threat-modeling/README.md) y responde:

1. La amenaza #4 (credenciales del Storage expuestas) — ¿en qué fila de esta tabla de responsabilidades cae? *(Respuesta esperada: Identidades y control de acceso → siempre es responsabilidad del Cliente, sin importar el modelo.)*
2. Si en vez de un App Service hubieras elegido una VM para alojar la aplicación, ¿qué filas adicionales de responsabilidad del Cliente aparecerían, y qué nuevas amenazas STRIDE tendrías que agregar a tu modelo? (Ej.: parches de SO sin aplicar → Elevation of Privilege por una vulnerabilidad conocida sin parchar)

---

## ✅ Checklist de la Sección 5

- [ ] Exploraste el formulario completo de creación de una VM sin crearla
- [ ] Exploraste el formulario completo de creación de un App Service sin crearlo
- [ ] Confirmaste que tu Resource Group sigue vacío
- [ ] Completaste la tabla de responsabilidades de las 8 capas para los 3 modelos
- [ ] Relacionaste al menos una fila de la tabla con una amenaza de tu modelo STRIDE

---

## 🧠 Preguntas de repaso

1. ¿Por qué el formulario de creación de una VM tiene más campos relacionados con seguridad de infraestructura que el de un App Service?
2. Menciona dos configuraciones de seguridad que **siempre** seguirán siendo tu responsabilidad, sin importar si eliges IaaS, PaaS o SaaS.
3. ¿Por qué elegimos App Service (PaaS) y no una VM (IaaS) para la Managed Identity de la Sección 6?

---

➡️ Continúa con la [Sección 6 — Managed Identity](../06-managed-identity/README.md)
