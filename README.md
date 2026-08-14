# ☁️ Ciberseguridad en la Nube

**Especialización en Ciberseguridad — Escuela de Comunicaciones Militares (ESCOM)**

Curso de especialización en modalidad **híbrida** (presencial + virtual) enfocado en el diseño, implementación y operación de controles de seguridad en entornos de cómputo en la nube, con laboratorios prácticos sobre **Microsoft Azure**.

| | |
|---|---|
| **Modalidad** | Híbrida (presencial + virtual) |
| **Duración** | 5 sesiones · 32 h trabajo en clase (HTA) + 64 h trabajo independiente (HTI) = **96 h totales** |
| **Plataforma de laboratorios** | Microsoft Azure (Free Tier) |
| **Créditos académicos** | 2 |
| **Docente** | Manuel Alejandro Vargas Rojas |

---

## 📌 Descripción general

Este curso dota al estudiante de las competencias técnicas y de gobernanza necesarias para operar de forma segura en entornos cloud, siguiendo una progresión pedagógica que avanza de lo conceptual hacia lo técnico-operativo:

1. **Fundamentos y principios de seguridad** → 2. **Gobernanza y riesgo** → 3. **Identidad y datos** → 4. **Infraestructura y redes** → 5. **Operaciones y respuesta a incidentes**

Cada sesión combina un bloque **teórico** (presentación de fundamentos y marcos de referencia) con un **laboratorio práctico guiado** sobre Azure, reforzando en todo el curso los principios transversales de **Zero Trust, Least Privilege y Defense in Depth**.

### 🎯 Competencia general

El estudiante estará en capacidad de diseñar, implementar y evaluar controles de seguridad en entornos de cómputo en la nube, aplicando los principios de responsabilidad compartida, gestión de riesgos, gobernanza y operación segura.

### Competencias específicas

- Aplicar el modelo de responsabilidad compartida y los principios de Zero Trust, Defense in Depth y Least Privilege en el diseño de arquitecturas cloud.
- Evaluar el cumplimiento normativo y el riesgo de un entorno cloud utilizando marcos de gobernanza (SOC 2, ISO 31000/NIST RMF) y herramientas CSPM.
- Configurar mecanismos de identidad y acceso (IAM, RBAC/ABAC, federación SAML/OIDC/JWT) y de protección de datos (cifrado, KMS, DLP) en la nube.
- Diseñar e implementar arquitecturas de red seguras con segmentación, firewall/IDS-IPS, Private Endpoints y WAF.
- Operar capacidades de monitoreo, detección y respuesta a incidentes de seguridad en entornos cloud.

### 👥 Audiencia y requisitos

Audiencia mixta: perfiles de TI con poca exposición a seguridad y perfiles de seguridad con poca exposición a cloud. Se recomiendan conocimientos básicos de redes, sistemas operativos y fundamentos de seguridad de la información (CIA). No se requiere experiencia previa en proveedores cloud.

### 🛠️ Requisitos técnicos para los laboratorios

- Cuenta de [Microsoft Azure Free Tier](https://azure.microsoft.com/free/)
- Navegador web actualizado + acceso a Azure Portal y Azure Cloud Shell
- [OWASP Threat Dragon](https://owasp.org/www-project-threat-dragon/) (web o desktop)
- [Prowler](https://github.com/prowler-cloud/prowler) y [CloudSploit](https://github.com/aquasecurity/cloudsploit)
- Git / GitHub para clonar las guías de laboratorio

---

## 🗂️ Estructura del curso

| Sesión | Tema | HTA | HTI | Laboratorio |
|---|---|---|---|---|
| 1 | Fundamentos de Cloud Computing, Responsabilidad Compartida y Principios de Seguridad | 7 h | 13 h | Threat Modeling + Managed Identity |
| 2 | Gobernanza, Riesgo, Cumplimiento y Arquitectura Segura | 7 h | 13 h | CSPM con Prowler y CloudSploit |
| 3 | Seguridad de Datos e Identidad y Acceso (IAM) | 6 h | 13 h | IAM, cifrado y federación de identidad |
| 4 | Seguridad de Infraestructura, Redes y Acceso Privado | 6 h | 13 h | Red segura + PaaS + WAF |
| 5 | Cloud Security Operations, Observabilidad y Respuesta a Incidentes | 6 h | 12 h | Respuesta a incidente de credenciales IAM |
| **Total** | | **32 h** | **64 h** | |

---

## 📖 Contenido y laboratorios por sesión

### Sesión 1 — Fundamentos de Cloud Computing, Responsabilidad Compartida y Principios de Seguridad

**Contenido teórico:**
- Modelos de servicio (IaaS/PaaS/SaaS) y de despliegue
- Shared Responsibility Model y panorama de amenazas (*Top Threats to Cloud Computing 2024*)
- Principios fundamentales: Least Privilege, Defense in Depth, Zero Trust, Trust Boundaries
- Threat Modeling, STRIDE, OWASP

**Laboratorio:**
- Alta y exploración de una suscripción Azure Free Tier (Portal, Resource Groups)
- Ejercicio de **Threat Modeling con OWASP Threat Dragon** sobre un escenario de arquitectura cloud
- Creación de una **Managed Identity** y uso para integrar 2 componentes cloud sin credenciales embebidas

**Evidencia:** Diagrama de responsabilidad compartida, modelo de amenazas en Threat Dragon y Managed Identity funcional.

---

### Sesión 2 — Gobernanza, Riesgo, Cumplimiento y Arquitectura Segura

**Contenido teórico:**
- Marcos de cumplimiento SOC (tipos de reporte SOC 1/2/3, foco en **SOC 2 Tipo II**)
- Gestión de riesgos: conceptos base, ciclo **ISO 31000 / NIST RMF**, gestión de riesgos en la nube
- Principios de diseño seguro: **CSA Guidance**, Security by Design, Security by Default, Control Plane vs. Data Plane

**Laboratorio:**
- Configuración de gobernanza con Azure Policy
- **CSPM (Cloud Security Posture Management) con Prowler y CloudSploit** sobre el entorno Azure del curso

**Evidencia:** Informe de gobernanza/riesgo y reporte CSPM con hallazgos priorizados.

---

### Sesión 3 — Seguridad de Datos e Identidad y Acceso (IAM)

**Contenido teórico:**
- **Cloud IAM:** IAM, RBAC, ABAC, MFA, Federation, SAML, OAuth 2.0, OpenID Connect, JWT
- Protección de datos: cifrado en tránsito/reposo, KMS, Secrets Management, Key Rotation, Data Classification, Tokenización, DLP

**Laboratorio:**
- Configuración de Microsoft Entra ID, RBAC y Azure Key Vault; cifrado de un Storage Account
- **Integración SAML** de Entra ID con [sptest.iamshowcase.com](https://sptest.iamshowcase.com/)
- **Integración OIDC** con la aplicación de ejemplo [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp)

**Evidencia:** Entorno IAM configurado + integraciones SAML/OIDC funcionales.

---

### Sesión 4 — Seguridad de Infraestructura, Redes y Acceso Privado

**Contenido teórico:**
- Arquitectura de red segura, Private Endpoints, Security Groups, WAF, Bastion, TLS
- Acceso privado a servicios PaaS

**Laboratorio** *(guía consolidada — ver `/sesion-4/`)*:
- **Parte I:** Arquitectura Hub & Spoke con Zentyal (NVA/IDS-IPS) y NSG por subred
- **Parte II:** Aplicación PaaS de 3 capas (App Service + Function App + Azure SQL) con Private Endpoints y Managed Identity
- **Parte III:** Contenedor DVWA + **Application Gateway con WAF (OWASP CRS)**, con prueba de bloqueo de SQLi/Command Injection

**Evidencia:** Arquitectura de red segura desplegada con bloqueo de ataques verificado.

---

### Sesión 5 — Cloud Security Operations, Observabilidad y Respuesta a Incidentes

**Contenido teórico:**
- **Módulo 1 — Visibilidad:** Logging, Monitoring, Telemetría, Observabilidad (Azure Monitor/Sentinel, CloudTrail, GuardDuty)
- **Módulo 2 — Detección:** MITRE ATT&CK Cloud, Threat Hunting, SIEM, XDR, SOAR
- **Módulo 3 — Respuesta:** Playbooks, forense cloud, Contención, RCA, Lessons Learned
- **Tendencias:** CSPM, IA Security, SASE, CNAPP, Post-Quantum Security, Cloud Native Security

**Laboratorio:**
- **Escenario de compromiso de credenciales IAM:** análisis de logs → identificación del ataque → contención → erradicación → informe ejecutivo

**Evidencia:** Informe ejecutivo del incidente.

---

## 📊 Evaluación

Cada laboratorio equivale al **20%** de la nota final (5 sesiones × 20% = 100%).

## 📚 Bibliografía

**Lecturas obligatorias**
- Edwards, J. *Cloud Security Fundamentals*.
- Cloud Security Alliance (CSA). *Top Threats to Cloud Computing 2024* (Egregious Eleven).

**Lecturas complementarias**
- Thompson, G. *Certificate of Cloud Security Knowledge (CCSK v5) Official Study Guide*.
- Dotson, C. *Practical Cloud Security: A Guide for Secure Design and Deployment*, 2nd Edition. O'Reilly Media.
- Messier, R. *Learning Cloud Security: Cloud Computing and Security Architecture Essentials*. O'Reilly Media.
- *Cloud Computing Security: Strategies and Best Practices*.

**Recursos adicionales**
- [OWASP Threat Dragon Documentation](https://owasp.org/www-project-threat-dragon/)
- [MITRE ATT&CK for Cloud](https://attack.mitre.org/matrices/enterprise/cloud/)

## 🗃️ Recursos del curso

- Google Classroom del módulo (materiales, foros, seguimiento HTI)
- Cuenta Microsoft Azure Free Tier
- GitHub (este repositorio) para guías de laboratorio versionadas
- OWASP Threat Dragon, Prowler, CloudSploit, Microsoft Entra ID, Azure Monitor/Sentinel

---

## 📁 Estructura sugerida del repositorio

```
.
├── README.md
├── sesion-1-fundamentos/
│   ├── presentacion/
│   └── laboratorio/
├── sesion-2-gobernanza-riesgo/
│   ├── presentacion/
│   └── laboratorio/
├── sesion-3-iam-datos/
│   ├── presentacion/
│   └── laboratorio/
├── sesion-4-infraestructura-redes/
│   ├── presentacion/
│   └── laboratorio/
└── sesion-5-secops-respuesta/
    ├── presentacion/
    └── laboratorio/
```

---

**Docente:** Manuel Alejandro Vargas Rojas · [manuelvargasrojas@cedoc.edu.co](mailto:manuelvargasrojas@cedoc.edu.co)
**Institución:** Escuela de Comunicaciones Militares (ESCOM) — Especialización en Ciberseguridad
