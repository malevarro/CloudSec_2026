# Sesión 5 — Cloud Security Operations, Observabilidad y Respuesta a Incidentes

**Especialización en Ciberseguridad · Escuela de Comunicaciones Militares (ESCOM)**
**Curso:** Seguridad en la Nube · **Instructor:** Manuel Alejandro Vargas Rojas (`manuelvargasrojas@cedoc.edu.co`)
**Sesión 5 de 5** — sesión de cierre de la especialización.

---

## 1. Ubicación en el curso

```
1. Fundamentos y Responsabilidad Compartida
2. Gobernanza, Riesgo, Cumplimiento y Arquitectura Segura
3. Datos e Identidad y Acceso (IAM)
4. Infraestructura, Redes y Acceso Privado
5. Security Operations, Observabilidad e Incidentes   ← estás aquí
```

Las Sesiones 1 a 3 construyeron la base conceptual (responsabilidad compartida, riesgo, gobernanza) y protegieron identidad y datos. La Sesión 4 aseguró la red y el acceso privado. Esta sesión **cierra el ciclo**: da visibilidad a todo lo construido, detecta cuando algo falla y responde ante un incidente real simulado.

---

## 2. Resultado de aprendizaje

> Opera capacidades de monitoreo, detección y respuesta a incidentes de seguridad en entornos cloud.

Al finalizar la sesión, el estudiante estará en capacidad de:

- Diferenciar logging, monitoring, telemetría y observabilidad, y diseñar una estrategia de visibilidad para un entorno cloud.
- Aplicar **MITRE ATT&CK for Cloud** para mapear técnicas de un ataque real y explicar el rol de **SIEM, XDR y SOAR** en su detección.
- Ejecutar el ciclo de respuesta a incidentes (contención, erradicación, RCA) ante un escenario de **compromiso de credenciales IAM**.
- Reconocer las tendencias que están redefiniendo la seguridad cloud: **CNAPP, SASE, IA Security y Post-Quantum**.

**Peso en la evaluación:** 20 % de la nota final del módulo (igual que las cuatro sesiones anteriores).

---

## 3. Contenido temático

| Bloque | Temas |
|---|---|
| Visibilidad | Logging, Monitoring, Telemetría, Observabilidad |
| Detección | MITRE ATT&CK Cloud, Threat Hunting, SIEM, XDR, SOAR |
| Respuesta | Playbooks, forense cloud, contención, RCA, lecciones aprendidas |
| Tendencias | CSPM→CNAPP, SASE, IA Security, Post-Quantum Security, Cloud Native Security |

---

## 4. Estructura de esta carpeta

```
Sesion-5-secops-respuesta/
├── README.md            ← este archivo
├── Presentacion/
│   └── Sesion5_SecOps_Observabilidad_Incidentes.pptx
└── Laboratorio/
    ├── README.md                             ← punto de partida del laboratorio
    ├── 01-Preparacion-y-Reactivacion.md
    ├── 02-Habilitar-Visibilidad.md
    ├── 03-Despliegue-WAF-DVWA.md
    ├── 04-Simulacion-Incidente.md
    ├── 05-Deteccion-KQL.md
    ├── 06-Contencion-y-Erradicacion.md
    ├── 07-RCA-e-Informe-Ejecutivo.md
    └── RUBRICA.md
```

### [`Presentacion/`](Presentacion/)

La presentación teórica de la sesión (38 diapositivas), en la identidad visual ESCOM confirmada para todo el curso. Cubre los cuatro bloques temáticos, la continuidad con las Sesiones 1 a 4, y el puente hacia el laboratorio.

### [`Laboratorio/`](Laboratorio/)

Guía de laboratorio guiado, en 7 secciones consecutivas, que **reutiliza la arquitectura desplegada en la Sesión 4** y le añade un WAF + DVWA, un pipeline de observabilidad, y la simulación controlada de un incidente de compromiso de credenciales IAM. Empieza por [`Laboratorio/README.md`](Laboratorio/README.md).

La entrega evaluada de esta sesión es el **informe ejecutivo del incidente**, construido a partir de la plantilla incluida en la última sección del laboratorio. Ver [`Laboratorio/RUBRICA.md`](Laboratorio/RUBRICA.md) para el detalle de calificación.

---

## 5. Prerrequisitos

- Haber completado la **Sesión 4** con su arquitectura Hub-and-Spoke aún desplegada (VM deallocadas, no eliminadas).
- Cuenta de **Azure Free Trial** (o con el crédito inicial de USD $200) con saldo disponible — ver la tabla de costos estimados en [`Laboratorio/README.md`](Laboratorio/README.md#5-costos-estimados-y-control-del-crédito).
- Rol de **Owner** sobre la suscripción y **Global Administrator** sobre el tenant de Microsoft Entra ID por defecto.

---

## 6. Entrega

- **Formato:** un único archivo PDF.
- **Canal:** Google Classroom, tarea de la Sesión 5.
- Cualquier entrega que no cumpla este formato se califica con 0 — ver la regla completa en [`Laboratorio/RUBRICA.md`](Laboratorio/RUBRICA.md).

---

## 7. Bibliografía de la sesión

**Obligatorias**
- Edwards, J. *Cloud Security Fundamentals.*
- Thompson, G. *Certificate of Cloud Security Knowledge (CCSK v5) Official Study Guide.*

**Complementarias**
- Messier, R. *Learning Cloud Security: Cloud Computing and Security Architecture Essentials.*
- Dotson, C. *Practical Cloud Security: A Guide for Secure Design and Deployment*, 2nd Edition.
- Cloud Security Alliance. *Top Threats to Cloud Computing 2024.*

**Recursos técnicos**
- MITRE ATT&CK for Cloud — attack.mitre.org/matrices/enterprise/cloud
- Documentación de Azure Monitor, Log Analytics y Microsoft Sentinel — learn.microsoft.com
- NIST SP 800-61 — Computer Security Incident Handling Guide

---

Con esta sesión se cierra el recorrido de la especialización **Seguridad en la Nube**.
