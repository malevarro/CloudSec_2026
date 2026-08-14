[⬅ Volver al índice](../README.md)

# Sección 2 — Alta en Azure Free Tier

**Tiempo estimado:** 15–20 minutos

## 🎯 Objetivo de esta sección

Crear tu cuenta de Azure en el nivel gratuito (**Free Trial**) y configurar dos controles de seguridad/costo básicos antes de crear cualquier recurso: una **alerta de presupuesto** y la **verificación en dos pasos (MFA)** de tu cuenta.

---

## ✅ Requisitos que debes tener a la mano

- Un correo electrónico (con o sin cuenta Microsoft existente — si no tienes una, la crearemos).
- Un número de teléfono celular propio, capaz de recibir SMS o llamada.
- Una tarjeta débito o crédito **no prepago** a tu nombre (o de un adulto responsable que lo autorice). Se usa solo para verificación de identidad.

> ⚠️ Azure permite **una sola cuenta Free Trial por persona**. Si ya usaste el beneficio gratuito anteriormente con tu cuenta Microsoft, deberás usar una cuenta distinta o contactar al instructor.

---

## Paso 1 — Ir al formulario de registro

### ✅ 1.1

1. Abre tu navegador y ve a: **https://azure.microsoft.com/free**
2. Haz clic en el botón **Start free** (Comenzar gratis) o **Try Azure for free**.

### ✅ 1.2 Iniciar sesión o crear una cuenta Microsoft

Azure requiere una **cuenta Microsoft** (la misma que usarías para Outlook, Xbox o Microsoft 365) para administrar la suscripción.

**Si ya tienes una cuenta Microsoft** (correo `@outlook.com`, `@hotmail.com`, `@live.com`, o cualquier correo que hayas registrado antes en un servicio Microsoft):
1. Ingresa tu correo y contraseña cuando el sitio te lo solicite.
2. Continúa en el **Paso 2** de esta guía.

**Si NO tienes una cuenta Microsoft:**
1. En la pantalla de inicio de sesión, haz clic en **Crear una** o **Create one**.
2. Puedes usar cualquier correo electrónico existente (Gmail, Yahoo, institucional, etc.) o crear uno nuevo `@outlook.com`.
3. Si usas un correo existente: escríbelo y haz clic en **Siguiente**.
4. Crea una **contraseña segura** (mínimo 8 caracteres, combinando mayúsculas, minúsculas, números y símbolos).
5. Completa tu **nombre y país/región**.
6. Completa tu **fecha de nacimiento** (Azure requiere confirmar que eres mayor de edad según las leyes locales; si tu curso es institucional y tienes dudas, consulta con tu instructor).
7. Verifica tu correo: Microsoft enviará un código de un solo uso al correo que registraste. Ingresa ese código en la pantalla.
8. Verifica que no eres un robot resolviendo el desafío que se muestre (puzzle o captcha).

### 🧪 Checkpoint

Deberías estar ahora en la pantalla de **"Regístrate en Azure gratis"** (Sign up for Azure), con tu cuenta Microsoft ya identificada en la parte superior.

---

## Paso 2 — Completar el formulario de la cuenta Azure

Azure te pedirá completar 4 secciones. Avanza sección por sección; cada una tiene un botón **Siguiente / Next**.

### ✅ 2.1 Identity (Identidad)

1. Confirma o completa tu **nombre**, **apellido** y **correo electrónico**.
2. Ingresa tu **número de teléfono**.
3. Haz clic en **Enviar código / Send code**. Recibirás un SMS con un código de 6 dígitos.
4. Ingresa el código recibido y haz clic en **Verificar código / Verify code**.

### ✅ 2.2 Identity verification by card (Verificación de identidad con tarjeta)

1. Ingresa los datos de tu tarjeta débito o crédito **no prepago**: número, fecha de vencimiento, código de seguridad (CVV) y nombre del titular.
2. Ingresa la dirección de facturación asociada a la tarjeta.

> ⚠️ **Esto es solo verificación de identidad.** Azure puede colocar una retención temporal de **USD 1** que se libera automáticamente en pocos días. **No se te cobrará nada** salvo que tú mismo decidas más adelante cambiar a una suscripción de pago (`Pay as you go`), algo que **no haremos en este laboratorio**.

### ✅ 2.3 Agreement (Acuerdo)

1. Lee (o al menos revisa) el **Customer Agreement** y el **Offer Details** (detalles de la oferta gratuita).
2. Marca la casilla de aceptación.
3. Haz clic en **Sign up / Registrarse**.

### ✅ 2.4 Confirmación

1. Azure mostrará una pantalla de bienvenida indicando que tu suscripción fue creada, generalmente con el nombre **"Azure subscription 1"** o **"Free Trial"**.
2. Haz clic en **Go to the portal** (Ir al portal) o **Explorar Azure**.

### 🧪 Checkpoint

Deberías llegar al **Azure Portal** (`portal.azure.com`) con tu sesión iniciada. En la esquina superior derecha debe aparecer tu nombre o inicial.

### 📸 Evidencia recomendada

Captura de pantalla de la página de confirmación de la suscripción ("¡Tu suscripción está lista!" o similar).

---

## Paso 3 — Verificar el tipo y estado de tu suscripción

Confirmemos que efectivamente quedaste en el nivel gratuito antes de continuar.

### ✅ 3.1

1. En el Azure Portal, usa la barra de búsqueda superior (el campo con el ícono de lupa) y escribe: `Subscriptions`
2. Haz clic en el resultado **Subscriptions** (Suscripciones).
3. Deberías ver una fila con tu suscripción. Verifica que la columna **Offer** o **Tipo de oferta** diga algo como **"Free Trial"** o **"Azure subscription 1"**.
4. Haz clic sobre el nombre de la suscripción para ver el detalle. Anota el valor de **Subscription ID** (lo usarás ocasionalmente para identificar tu entorno).

### 🧪 Checkpoint

El estado de la suscripción debe decir **Active** (Activa).

---

## Paso 4 — Configurar una alerta de presupuesto (obligatorio)

Este es el control de seguridad financiera más importante del laboratorio: nos avisará por correo si el gasto se acerca a un límite, **antes** de que ocurra cualquier cargo.

### ✅ 4.1 Ir a Cost Management

1. En la barra de búsqueda superior, escribe: `Cost Management`
2. Haz clic en **Cost Management + Billing**.
3. En el menú lateral izquierdo, selecciona tu suscripción (aparecerá bajo **Billing scopes** o similar) y luego haz clic en **Cost Management**.

### ✅ 4.2 Crear un presupuesto (Budget)

1. En el menú lateral, dentro de Cost Management, selecciona **Budgets**.
2. Haz clic en **+ Add** (Agregar).
3. Completa el formulario:
   - **Name:** `presupuesto-lab1`
   - **Reset period:** `Monthly`
   - **Expiration date:** deja la fecha por defecto (usualmente 1 año adelante).
   - **Amount:** escribe `10` (USD 10 — muy por encima de lo que este laboratorio va a consumir, pero suficientemente bajo para detectar cualquier anomalía).
4. Haz clic en **Next** (Siguiente).
5. En la sección de alertas, configura al menos:
   - Una alerta al **50 %** del presupuesto (`% of budget = 50`).
   - Una alerta al **90 %** del presupuesto.
6. En **Alert recipients (email)**, escribe tu propio correo electrónico.
7. Haz clic en **Create** (Crear).

### 🧪 Checkpoint

El nuevo presupuesto `presupuesto-lab1` debe aparecer en la lista de **Budgets** con estado activo y el monto de USD 10.

### 📸 Evidencia recomendada

Captura de pantalla del presupuesto creado, mostrando el nombre, el monto y las alertas configuradas.

---

## Paso 5 — Habilitar verificación en dos pasos (MFA) en tu cuenta

Antes de seguir construyendo, reforcemos la identidad de tu propia cuenta — el mismo principio de **Zero Trust** que viste en la teoría ("verificar explícitamente") aplica también a tu cuenta de administrador.

### ✅ 5.1

1. Abre una nueva pestaña y ve a: **https://mysignins.microsoft.com/security-info**
2. Inicia sesión con la misma cuenta Microsoft que usaste para Azure, si te lo pide.
3. Haz clic en **+ Agregar método de inicio de sesión** (Add sign-in method).
4. Selecciona **Aplicación autenticadora** (Authenticator App) si tienes un teléfono para instalar Microsoft Authenticator, o **Teléfono** para recibir un código por SMS como alternativa más simple.
5. Sigue el asistente en pantalla (si eliges Authenticator, deberás instalar la app **Microsoft Authenticator** desde la tienda de aplicaciones de tu teléfono y escanear un código QR).
6. Confirma el método completando la verificación de prueba que te solicite el asistente.

### 🧪 Checkpoint

En la lista de **Métodos de inicio de sesión**, debe aparecer al menos un método adicional a tu contraseña (Authenticator o Teléfono).

---

## ✅ Checklist de la Sección 2

- [ ] Cuenta Microsoft creada o identificada
- [ ] Suscripción de Azure Free Trial creada y en estado **Active**
- [ ] Presupuesto `presupuesto-lab1` de USD 10 configurado con alertas al 50 % y 90 %
- [ ] Verificación en dos pasos (MFA) habilitada en tu cuenta
- [ ] Subscription ID anotado para referencia futura

---

## 🧠 Preguntas de repaso

1. ¿Por qué Azure solicita una tarjeta si el laboratorio completo es gratuito?
2. ¿En qué se diferencia una alerta de presupuesto de un límite de gasto forzado (spending limit)? *(Pista: una alerta te notifica, pero no detiene el consumo por sí sola.)*
3. ¿Cómo conecta la activación de MFA en tu cuenta con el principio "verificar explícitamente" de Zero Trust visto en la teoría?

---

➡️ Continúa con la [Sección 3 — Exploración del Portal de Azure](../03-exploracion-portal/README.md)
