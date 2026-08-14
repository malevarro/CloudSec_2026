# Laboratorio — Sesión 1: Fundamentos de Cloud Computing, Responsabilidad Compartida y Principios de Seguridad

**Especialización en Ciberseguridad · Seguridad en la Nube**
**Duración estimada:** 90–120 minutos
**Modalidad:** individual, sobre una suscripción **Azure Free Tier**

> 📌 Esta guía asume que **no tienes experiencia previa con Azure**. Cada paso está descrito de forma explícita, incluyendo nombres exactos de botones y menús. Si en algún punto la interfaz de Azure no coincide exactamente con lo descrito (Microsoft actualiza el portal con frecuencia), busca la opción equivalente más cercana — el objetivo del paso siempre se explica antes del "cómo".

---

## 🎯 Objetivos del laboratorio

Al finalizar esta guía habrás:

1. Construido un **modelo de amenazas** con OWASP Threat Dragon sobre un escenario cloud simple, usando la metodología STRIDE.
2. Creado tu **cuenta de Azure Free Tier** y configurado controles básicos de gasto.
3. Explorado el **Portal de Azure** y sus componentes principales.
4. Comprendido y usado **Azure Resource Manager (ARM)** creando un Resource Group.
5. Mapeado las **responsabilidades de seguridad** entre proveedor y cliente comparando IaaS, PaaS y SaaS.
6. Creado una **Managed Identity** y la usaste para que un App Service acceda a un Storage Account y a un Key Vault **sin ninguna credencial embebida**.

Este laboratorio corresponde a la evidencia de aprendizaje de la Sesión 1 (20 % de la nota del módulo): diagrama de responsabilidad compartida, modelo de amenazas en OWASP Threat Dragon y Managed Identity funcional integrando 2 componentes cloud.

---

## ⚠️ Antes de empezar: reglas de oro para no gastar dinero

Esta guía está diseñada **100 % para la capa gratuita de Azure (Free Tier / Free Trial)**. Aun así, Azure es una plataforma de pago por uso y es responsabilidad tuya evitar cargos. Sigue estas reglas:

| Regla | Por qué |
|---|---|
| Usa **siempre** los tamaños/SKU marcados como `Free (F1)`, `Standard LRS` o equivalentes indicados en esta guía | Son los niveles sin costo o de costo mínimo cubiertos por el crédito gratuito |
| Configura una **alerta de presupuesto** (Sección 2) antes de crear cualquier recurso | Te avisa por correo si te acercas a gastar el crédito |
| **Elimina el Resource Group completo al terminar** (última tarea de la Sección 6) | Borra todos los recursos del laboratorio de una sola vez y evita cargos residuales |
| No selecciones "Pay as you go" ni agregues métodos de pago adicionales durante el laboratorio | La cuenta Free Trial ya incluye todo lo necesario |
| Si Azure te pide seleccionar una réplica/redundancia, elige siempre la opción más económica (`LRS`) | Es suficiente para un entorno de laboratorio |

> 💰 La cuenta Free de Azure incluye: **USD 200 en crédito** utilizable durante los primeros 30 días, **más de 20 servicios populares gratis durante 12 meses** (incluye App Service en el nivel `F1`), y **más de 65 servicios "siempre gratis"** dentro de límites mensuales. Los recursos que crearemos en esta guía (App Service F1, una Storage Account pequeña, un Key Vault con pocas operaciones) caben cómodamente dentro de estos límites.

---

## ✅ Prerrequisitos

Antes de iniciar la Sección 2, ten a la mano:

- [ ] Un **correo electrónico** propio (puede ser cualquiera; si no tienes cuenta Microsoft, la crearás en la Sección 2).
- [ ] Un **número de teléfono celular propio** capaz de recibir SMS o llamadas (para verificación de identidad).
- [ ] Una **tarjeta débito o crédito NO prepago** a tu nombre (o de un adulto responsable). Azure la usa **únicamente para verificar que eres una persona real**; puede aparecer una retención temporal de USD 1 que se revierte automáticamente. **No se te cobra nada** mientras permanezcas en el nivel gratuito.
- [ ] Un **navegador web actualizado** (Google Chrome, Microsoft Edge o Firefox).
- [ ] **OWASP Threat Dragon (versión de escritorio)** — lo instalarás en la Sección 1; no requiere cuenta de GitHub.
- [ ] Un editor de texto simple (Bloc de notas, VS Code, o similar) para tomar notas y guardar fragmentos de configuración.
- [ ] Opcional: una cuenta de **GitHub**, si vas a publicar tu propia copia de esta guía o tus evidencias.

---

## 🏷️ Convención de nombres que usaremos

Azure exige que ciertos nombres (Storage Account, Web App, Key Vault) sean **globalmente únicos** en todo Azure — es decir, nadie más en el mundo puede tener ese mismo nombre. Para evitar choques, reemplaza `<inic>` por tus iniciales (ej. `jsr`) y `<num>` por un número corto que elijas (ej. tu año de nacimiento o `2026`) en todos los nombres de esta guía.

| Recurso | Patrón de nombre | Ejemplo |
|---|---|---|
| Resource Group | `rg-lab1-<inic>` | `rg-lab1-jsr` |
| App Service (Web App) | `app-lab1-<inic><num>` | `app-lab1-jsr2026` |
| Storage Account | `stlab1<inic><num>` (todo minúscula, sin guiones) | `stlab1jsr2026` |
| Key Vault | `kv-lab1-<inic><num>` | `kv-lab1-jsr2026` |
| Región recomendada | La más cercana a tu ubicación con capa gratuita disponible | `East US`, `Brazil South`, `West Europe`, etc. |

> 💡 Anota tus nombres exactos apenas los definas — los usarás repetidamente a lo largo de la guía.

---

## 🗂️ Estructura de esta guía

```text
lab-sesion1-fundamentos-cloud/
├── README.md                          ← estás aquí
├── 01-threat-modeling/README.md       → Threat Modeling con OWASP Threat Dragon
├── 02-alta-azure-free-tier/README.md  → Alta en Azure Free Tier
├── 03-exploracion-portal/README.md    → Exploración del Portal de Azure
├── 04-arm-resource-groups/README.md   → Azure Resource Manager / Resource Groups
├── 05-mapeo-responsabilidades/README.md → Mapeo de responsabilidades por servicio
└── 06-managed-identity/README.md      → Managed Identity: App Service → Storage + Key Vault
```

Sigue las secciones **en este orden**: el modelo de amenazas de la Sección 1 describe la misma arquitectura que construirás en la Sección 6, y las secciones 2 a 5 preparan el terreno (cuenta, portal, organización de recursos y responsabilidades) antes de construir nada.

| # | Sección | Qué vas a hacer |
|---|---|---|
| 1 | [Threat Modeling con OWASP Threat Dragon](01-threat-modeling/README.md) | Diagramar el escenario y aplicar STRIDE |
| 2 | [Alta en Azure Free Tier](02-alta-azure-free-tier/README.md) | Crear tu cuenta y configurar alertas de gasto |
| 3 | [Exploración del Portal](03-exploracion-portal/README.md) | Familiarizarte con la interfaz de Azure |
| 4 | [Azure Resource Manager / Resource Groups](04-arm-resource-groups/README.md) | Crear y organizar tu primer Resource Group |
| 5 | [Mapeo de responsabilidades por servicio](05-mapeo-responsabilidades/README.md) | Comparar VM vs. App Service vs. SaaS |
| 6 | [Managed Identity](06-managed-identity/README.md) | Integrar App Service con Storage y Key Vault sin credenciales |

---

## 🔑 Convenciones usadas en esta guía

| Símbolo | Significado |
|---|---|
| ✅ | Paso a ejecutar |
| ⚠️ | Advertencia importante (costos, seguridad, o errores comunes) |
| 💡 | Tip o explicación conceptual adicional |
| 🧪 | Punto de verificación ("checkpoint") — confirma esto antes de seguir |
| 📸 | Evidencia recomendada para tu entrega (captura de pantalla) |

---

## 🧹 Al terminar el laboratorio

La última tarea de la [Sección 6](06-managed-identity/README.md) es **eliminar el Resource Group completo**. No cierres esta guía sin haber completado ese paso — es la forma más simple y segura de asegurarte de que no queden recursos generando cargos.

---

## 📚 Recursos de referencia

- [Azure Free Account — preguntas frecuentes](https://azure.microsoft.com/free/free-account-faq/)
- [OWASP Threat Dragon — documentación](https://docs.threatdragon.org/)
- [Documentación de Azure Resource Manager](https://learn.microsoft.com/azure/azure-resource-manager/management/overview)
- [Managed identities for Azure resources](https://learn.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
- [Shared responsibility in the cloud](https://learn.microsoft.com/azure/security/fundamentals/shared-responsibility)
