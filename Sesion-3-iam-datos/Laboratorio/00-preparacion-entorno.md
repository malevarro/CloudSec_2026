[⬅ Volver al README](./README.md)

# 0. Preparación del entorno

## 🎯 Objetivo

Antes de tocar cualquier recurso de Azure, vamos a: verificar tu cuenta, **configurar una alerta de presupuesto** (para que nunca te lleves una sorpresa de facturación), instalar las herramientas necesarias en tu computador, y crear el grupo de recursos donde vivirá todo el laboratorio.

> Si ya hiciste el laboratorio de la Sesión 1 o la Sesión 2 de este curso, probablemente ya tienes tu cuenta Azure Free Tier, PowerShell y el módulo Az instalados. Puedes saltar directamente al **Paso 4** (crear el grupo de recursos). Aun así, léelo por encima para confirmar que no te falta nada nuevo (Python y Git son nuevos para esta sesión).

---

## Prerrequisitos

- Un computador con Windows, macOS o Linux, con permisos de administrador para instalar software.
- Una dirección de correo electrónico (personal o institucional).
- Una tarjeta de crédito o débito **solo para fines de verificación de identidad** — Azure Free Tier no cobra nada mientras te mantengas dentro de los límites gratuitos, y este laboratorio está diseñado exactamente para eso.

---

## Paso 1 — Crear o verificar tu cuenta Azure Free Tier

1. Abre tu navegador y ve a [azure.microsoft.com/free](https://azure.microsoft.com/free).
2. Haz clic en el botón **"Start free"** / **"Comenzar gratis"**.
3. Inicia sesión con una cuenta Microsoft existente, o crea una nueva si no tienes.
4. Completa el formulario de verificación (identidad y tarjeta). Azure te informará claramente que no se te cobrará nada sin tu autorización explícita.
5. Espera a que el portal termine de aprovisionar tu suscripción — te redirigirá automáticamente a [portal.azure.com](https://portal.azure.com).
6. En la esquina superior derecha del Portal, haz clic en tu nombre/correo para confirmar que ves el texto **"Free Trial"** o **"Azure subscription 1"** como nombre de tu suscripción.

📸 **Captura para el informe — 0.1:** pantalla del Azure Portal mostrando tu suscripción activa (Portal > "Subscriptions" en el buscador superior > captura de la lista con el nombre y estado "Activo").

---

## Paso 2 — Configurar una alerta de presupuesto

Esto es **obligatorio**, no opcional. Nos protege de cualquier recurso olvidado.

1. En la barra de búsqueda superior del Portal, escribe `Cost Management` y selecciona **"Cost Management + Billing"**.
2. En el menú izquierdo, bajo tu suscripción, haz clic en **"Budgets"** (Presupuestos).
3. Haz clic en **"+ Add"** (Agregar).
4. Completa el formulario:
   - **Name:** `presupuesto-lab-nube`
   - **Reset period:** `Monthly`
   - **Amount:** `5` (USD) — un umbral bajo, suficiente para detectar cualquier cosa anómala.
5. En la sección **"Alert conditions"**, agrega una alerta al **80 %** del presupuesto (es decir, 4 USD), tipo "Actual".
6. En **"Alert recipients"**, agrega tu propio correo electrónico.
7. Haz clic en **"Create"** (Crear).

📸 **Captura para el informe — 0.2:** el presupuesto creado, visible en la lista de "Budgets" con su nombre, monto y periodo.

---

## Paso 3 — Instalar las herramientas necesarias

### 3.1 PowerShell 7 y el módulo Az

1. Descarga PowerShell 7 desde [learn.microsoft.com/powershell/scripting/install](https://learn.microsoft.com/powershell/scripting/install/installing-powershell) según tu sistema operativo, e instálalo con las opciones por defecto.
2. Abre **PowerShell 7** (no el PowerShell 5 que viene integrado en Windows — busca específicamente "PowerShell 7" en el menú de inicio).
3. Instala el módulo Az ejecutando:
   ```powershell
   Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force
   ```
4. Cuando termine, verifica la instalación:
   ```powershell
   Get-InstalledModule -Name Az
   ```
5. Conéctate a tu cuenta de Azure:
   ```powershell
   Connect-AzAccount
   ```
   Se abrirá tu navegador para iniciar sesión. Una vez autenticado, vuelve a PowerShell — deberías ver el nombre de tu suscripción impreso en la consola.

📸 **Captura para el informe — 0.3:** la salida de `Connect-AzAccount` en PowerShell mostrando el nombre de tu suscripción y tu tenant.

### 3.2 Python 3.10 o superior

1. Ve a [python.org/downloads](https://www.python.org/downloads/) y descarga la versión más reciente de Python 3.
2. Durante la instalación en Windows, **marca la casilla "Add python.exe to PATH"** antes de hacer clic en "Install Now" — es el error más común de esta sección si se te olvida.
3. Verifica la instalación abriendo una terminal nueva (PowerShell) y ejecutando:
   ```powershell
   python --version
   ```
   Debe mostrar `Python 3.10.x` o superior.
4. Verifica también que `pip` (el instalador de paquetes de Python) esté disponible:
   ```powershell
   pip --version
   ```

📸 **Captura para el informe — 0.4:** la salida de `python --version` y `pip --version`.

### 3.3 Git

1. Descarga Git desde [git-scm.com/downloads](https://git-scm.com/downloads) e instálalo con las opciones por defecto.
2. Verifica la instalación:
   ```powershell
   git --version
   ```

---

## Paso 4 — Crear el grupo de recursos del laboratorio

Todos los recursos de este laboratorio (excepto los objetos de Entra ID, que no pertenecen a un grupo de recursos) van a vivir dentro de **un único grupo de recursos**, para que sea trivial eliminarlos todos al final.

1. Define tu `<alias>` personal — un identificador corto y único para ti (ej. tus iniciales + un número: `mvr01`). **Vas a usar este mismo alias en todo el laboratorio.** Anótalo en un lugar visible.
2. En el Azure Portal, busca `Resource groups` en la barra superior y haz clic en el resultado.
3. Haz clic en **"+ Create"**.
4. Completa el formulario:
   - **Subscription:** tu suscripción Free Trial.
   - **Resource group:** `rg-lab3-<alias>` (reemplaza `<alias>` por el tuyo, ej. `rg-lab3-mvr01`).
   - **Region:** elige una región cercana con buena disponibilidad de recursos gratuitos, por ejemplo `East US` o `West Europe` (evita regiones muy restringidas en capacidad para VMs gratuitas).
5. Haz clic en **"Review + create"** y luego en **"Create"**.

📸 **Captura para el informe — 0.5:** el grupo de recursos recién creado, mostrando su nombre y región (pantalla "Overview" del grupo de recursos).

---

## Paso 5 — Anotar tus identificadores

Vas a necesitar estos dos valores más adelante (secciones SAML y OIDC). Anótalos ahora:

1. En el Portal, busca `Microsoft Entra ID` y entra a su página de **"Overview"**.
2. Copia y guarda:
   - **Tenant ID** (también llamado Directory ID).
   - **Primary domain** (algo como `tunombre.onmicrosoft.com`).

---

## ❓ Preguntas de repaso — Sección 0

1. ¿Por qué es importante configurar una alerta de presupuesto incluso en una cuenta "gratuita"?
2. ¿Cuál es la diferencia entre un *Tenant* de Entra ID y una *Subscription* de Azure?
3. ¿Por qué agrupamos todos los recursos del laboratorio en un único grupo de recursos?
4. En tus propias palabras, ¿qué garantiza Azure Free Tier que NO sucederá mientras te mantengas dentro de los límites gratuitos?

---

[➡ Continuar con la Sección 1 — Entra ID + RBAC](./01-entra-id-rbac.md)
