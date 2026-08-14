# Sesión 1 — Fundamentos de Cloud Computing, Responsabilidad Compartida y Principios de Seguridad

**Especialización en Ciberseguridad · Saber: Seguridad en la Nube**
**Ejército Nacional — Escuela de Comunicaciones Militares**
**Instructor:** Manuel Alejandro Vargas Rojas
**Modalidad:** Híbrida (bloque teórico + laboratorio práctico guiado)
**Duración:** 7 horas de acompañamiento directo (HTA) + trabajo independiente (HTI)
**Peso en la nota del módulo:** 20 %

---

## 📖 Descripción general

La adopción acelerada de servicios en la nube exige que los profesionales de seguridad comprendan los riesgos, controles y modelos de responsabilidad propios de estos entornos. Esta primera sesión sienta las bases conceptuales y prácticas de todo el módulo de **Seguridad en la Nube**: qué es la computación en la nube, cómo se reparte la responsabilidad de seguridad entre el proveedor y el cliente, cuáles son las amenazas más relevantes del panorama actual, y qué principios (Least Privilege, Defense in Depth, Zero Trust) y metodologías (Threat Modeling con STRIDE y el enfoque OWASP) se usan para diseñar arquitecturas cloud seguras.

La sesión combina una **exposición magistral** con un **laboratorio guiado paso a paso en Microsoft Azure** (nivel Free Tier), en el que cada estudiante construye — con sus propias manos — un modelo de amenazas y una arquitectura mínima que demuestra el principio de Zero Trust aplicado a la gestión de identidades.

---

## 🎯 Objetivos de aprendizaje

Al finalizar esta sesión, el estudiante estará en capacidad de:

1. Delimitar responsabilidades de seguridad entre proveedor y cliente según el modelo de servicio (IaaS / PaaS / SaaS).
2. Reconocer el panorama de amenazas actuales en la nube (CSA Top Threats to Cloud Computing 2024).
3. Aplicar los principios de **Least Privilege**, **Defense in Depth** y **Zero Trust** en el diseño de una arquitectura cloud.
4. Ejecutar un ejercicio de **threat modeling** usando la metodología **STRIDE** y el enfoque **OWASP** (con la herramienta OWASP Threat Dragon).
5. Diagramar el **modelo de responsabilidad compartida** y configurar una **Managed Identity** funcional que integre dos componentes cloud sin credenciales embebidas.

Estos objetivos corresponden directamente al resultado de aprendizaje del syllabus: *"Aplica el modelo de responsabilidad compartida y los principios Zero Trust/Least Privilege/Defense in Depth en el diseño de una arquitectura cloud."*

---

## 🗂️ Contenidos temáticos

| Bloque | Tema |
|---|---|
| 1 | Modelos de servicio (IaaS/PaaS/SaaS) y de despliegue en la nube |
| 2 | Modelo de responsabilidad compartida |
| 3 | Principales amenazas de la computación en la nube (CSA Top Threats 2024) |
| 4 | Principios de seguridad: Least Privilege, Defense in Depth, Zero Trust, Trust Boundaries |
| 5 | Threat Modeling: fundamentos, Data Flow Diagrams, metodología STRIDE y enfoque OWASP |
| 6 | Puente a la Sesión 2: conceptos base de gestión de riesgos (ISO 31000 / NIST RMF) |

---

## 🗃️ Estructura de este repositorio

```
sesion-1-fundamentos-cloud-computing/
├── README.md                              ← estás aquí
├── presentacion/
│   └── Sesion1_Fundamentos_Cloud_Security.pptx   → Diapositivas del bloque teórico (39 diapositivas)
└── laboratorio/
    └── lab-sesion1-fundamentos-cloud/
        ├── README.md                      → Índice y prerrequisitos del laboratorio
        ├── RUBRICA-EVALUACION.md          → Rúbrica de calificación y forma de entrega
        ├── 01-threat-modeling/            → Threat Modeling con OWASP Threat Dragon
        ├── 02-alta-azure-free-tier/       → Alta en Azure Free Tier
        ├── 03-exploracion-portal/         → Exploración del Portal de Azure
        ├── 04-arm-resource-groups/        → Azure Resource Manager / Resource Groups
        ├── 05-mapeo-responsabilidades/    → Mapeo de responsabilidades por servicio
        ├── 06-managed-identity/           → Managed Identity: App Service → Storage + Key Vault
        └── assets/                        → Plantillas y modelos de amenazas generados
```

---

## 🧭 Cómo usar este material

Sigue este orden para aprovechar la sesión correctamente:

1. **Revisa la** [`Presentación`](./Presentación/Sesion1_Fundamentos_Cloud_Security.pptx) durante o antes del bloque teórico. Cubre los 6 bloques temáticos listados arriba, con ejemplos aplicados y referencias cruzadas a las sesiones siguientes del módulo.
2. **Sigue la guía de laboratorio** en orden, comenzando por [`Laboratorio/README.md`](./Laboratorio/README.md). El laboratorio modela primero la amenaza (Sección 1 del lab) y luego construye la arquitectura real que la mitiga (Secciones 2 a 6) — por eso el orden de las secciones importa.
3. **Consulta la rúbrica antes de entregar** en [`Laboratorio/RUBRICA-EVALUACION.md`](./Laboratorio/RUBRICA-EVALUACION.md).


---

## 🧪 Evidencia de aprendizaje y evaluación

| Evidencia | Dónde se genera |
|---|---|
| Diagrama de responsabilidad compartida | Laboratorio, Sección 5 |
| Modelo de amenazas en OWASP Threat Dragon | Laboratorio, Sección 1 (actualizado al cierre de la Sección 6) |
| Managed Identity funcional integrando 2 componentes cloud | Laboratorio, Sección 6 |

**Peso de esta evidencia:** 20 % de la nota del módulo.

> ⚠️ **La entrega del laboratorio se realiza únicamente en formato PDF, cargado en Google Classroom.** No se reciben archivos en ningún otro formato. Todo el detalle está en la [Rúbrica de Evaluación](./Laboratorio/RUBRICA-EVALUACION.md).

---

## ✅ Prerrequisitos

- Conocimientos básicos de fundamentos de TI y de seguridad de la información (redes, sistemas operativos, principios de confidencialidad-integridad-disponibilidad).
- No se requiere experiencia previa con proveedores cloud.
- Para el laboratorio: cuenta de correo, número de teléfono propio, y una tarjeta débito o crédito no prepago (solo para verificación de identidad de la cuenta Azure Free Tier — no se genera ningún cobro). Ver el detalle completo en el [README del laboratorio](./Laboratorio/README.md).

---

## 🔜 Próxima sesión

**Sesión 2 — Gobernanza, Riesgo, Cumplimiento y Arquitectura Segura**, que profundiza en marcos de gobernanza (SOC 2), el ciclo completo de gestión de riesgos (ISO 31000 / NIST RMF) ya introducido en esta sesión, diseño seguro (CSA Guidance, Security by Design/Default) y la diferencia entre Control Plane y Data Plane — con un laboratorio de auditoría de postura de seguridad (CSPM) usando Prowler y CloudSploit.

---

## 📚 Referencias principales

- Edwards, J. *Cloud Security Fundamentals.* (Lectura obligatoria)
- Thompson, G. *Certificate of Cloud Security Knowledge (CCSK v5) Official Study Guide.*
- Dotson, C. *Practical Cloud Security: A Guide for Secure Design and Deployment*, 2nd Ed. O'Reilly.
- Messier, R. *Learning Cloud Security: Cloud Computing and Security Architecture Essentials.* O'Reilly.
- Cloud Security Alliance (CSA). *Top Threats to Cloud Computing 2024 (Egregious Eleven).*
- OWASP Foundation. [OWASP Threat Dragon Documentation](https://docs.threatdragon.org/)
- Microsoft Learn. [Shared responsibility in the cloud](https://learn.microsoft.com/azure/security/fundamentals/shared-responsibility) · [What is Zero Trust?](https://learn.microsoft.com/security/zero-trust/zero-trust-overview)
- MITRE ATT&CK for Cloud — https://attack.mitre.org/matrices/enterprise/cloud/
