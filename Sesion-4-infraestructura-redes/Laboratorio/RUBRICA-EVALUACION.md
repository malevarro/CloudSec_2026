[← Volver al README](README.md)

# Rúbrica de Evaluación — Laboratorio Sesión 4

> **Peso:** 20 % de la nota final del módulo
> **Formato de entrega:** un único archivo **PDF**, cargado en la tarea correspondiente de **Google Classroom**
> **Resultado de aprendizaje evaluado:** *Diseña e implementa una arquitectura de red segura con segmentación, firewall y acceso privado.* (El componente de WAF se evalúa en la rúbrica de la Sesión 5, que retoma esta misma arquitectura.)

Esta rúbrica es la misma que usará el docente para calificar. Revísala **antes** de empezar el laboratorio y otra vez **antes** de entregar.

---

## 1. Qué debe contener el PDF entregado

1. Portada con tu nombre completo y fecha.
2. Las **22 capturas de pantalla** solicitadas a lo largo de la guía (numeradas 1.1 a 5.1), cada una con una línea de texto identificando qué muestra.
3. Las respuestas a las **16 preguntas de repaso** (12 en las secciones de Introducción, Parte A y Parte B + 4 generales del cierre).
4. El checklist de finalización de la sección 4.1, copiado con cada casilla marcada.
5. La indicación explícita de qué opción de datos usaste en la Parte B (Azure SQL o Storage Account) y, si aplica, evidencia del error que te llevó a cambiar de opción.
6. La captura de la sección 4.3 confirmando que las cuatro VMs quedaron en estado `VM deallocated` al cerrar la sesión de trabajo.

> ❗ Un PDF sin todas las capturas requeridas, o sin las respuestas a las preguntas de repaso, se califica sobre la evidencia efectivamente presentada.

---

## 2. Distribución de puntaje (100 puntos)

### A. Arquitectura Hub-and-Spoke con Zentyal — 40 puntos

| Criterio | Puntos |
|---|---|
| Las 3 VNets y sus subredes están correctamente creadas, cada spoke con su `WorkloadSubnet`, y `AppGatewaySubnet` reservada | 5 |
| Zentyal Server instalado y configurado correctamente (interfaces External/Internal, IP forwarding en Azure) | 8 |
| Reglas de firewall de Zentyal (internas y externas) correctamente configuradas, con `Deny-All` al final de cada tabla | 6 |
| IDS/IPS (Suricata) habilitado, con evidencia real de detección de un escaneo `nmap` entre las dos VMs de spoke | 8 |
| VNet Peering y UDR configurados correctamente, con evidencia de que el tráfico efectivamente pasa por Zentyal (TTL y/o `curl ifconfig.me`) | 6 |
| SSH restringido a la IP del estudiante en Zentyal y en ambas VMs de spoke | 3 |
| NSG con microsegmentación deny-by-default en `AppIntegrationSubnet` | 4 |

### B. Acceso Privado a Servicios PaaS — 40 puntos

| Criterio | Puntos |
|---|---|
| Recurso de datos (SQL o Storage) y Key Vault con acceso público deshabilitado | 6 |
| Private Endpoints correctamente creados y en estado `Succeeded`, con zonas DNS privadas enlazadas | 9 |
| `VM-App` con identidad administrada del sistema habilitada y correctamente asignada al recurso de datos elegido | 9 |
| `VM-App` correctamente ubicada en `AppIntegrationSubnet`, con tráfico forzado por Zentyal (`AppIntegrationSubnet` asociada a `RT-Spoke-App`) | 6 |
| Regla `Allow-App-to-Datos` correctamente configurada y ordenada en el panel de Zentyal | 6 |
| Evidencia de resolución DNS privada desde la propia app (`nslookup` a IPs de `10.2.1.0/24`) | 4 |

### C. Comprensión conceptual (preguntas de repaso) — 12 puntos

| Criterio | Puntos |
|---|---|
| Respuestas a las 12 preguntas de repaso de sección (Introducción, Parte A, Parte B), correctas y en las propias palabras del estudiante | 8 |
| Respuestas a las 4 preguntas de repaso generales del cierre, con razonamiento propio y coherente | 4 |

### D. Higiene de laboratorio y control de costos — 8 puntos

| Criterio | Puntos |
|---|---|
| Las cuatro VMs (`FW-Zentyal`, `VM-Spoke-App`, `VM-Spoke-Data`, `VM-App`) quedan en estado `VM deallocated` al cerrar la sesión de trabajo, con evidencia | 5 |
| Nomenclatura de recursos consistente con la especificada en la guía a lo largo de todo el documento | 3 |

---

## 3. Escala de valoración por criterio

| Nivel | % del puntaje del criterio | Descripción |
|---|---|---|
| **Excelente** | 100 % | El criterio se cumple completamente, con evidencia clara y explicación correcta cuando aplica |
| **Aceptable** | 70 % | El criterio se cumple en lo esencial, con evidencia parcial o alguna imprecisión menor |
| **Insuficiente** | 40 % | Hay evidencia de intento, pero el resultado no funciona como se esperaba o falta evidencia clave |
| **No presentado** | 0 % | No hay evidencia del criterio en el documento entregado |

> ℹ️ **Sobre la elección de datos (SQL vs. Storage Account):** ambas opciones son **equivalentes** para efectos de calificación en la sección B — no se penaliza haber usado la Opción 2. Si intentaste la Opción 1 y falló por una limitación real de la suscripción, la evidencia de ese intento (captura del error) se considera parte legítima del criterio de "Private Endpoints correctamente creados", no un defecto.

---

## 4. Penalizaciones

| Situación | Penalización |
|---|---|
| Entrega en un formato distinto a PDF único | La entrega se considera no realizada (nota de 0) |
| Entrega después de la fecha límite sin excusa avalada por la Dirección | Según el reglamento general del módulo (ver syllabus) |
| Evidencia de que alguna de las cuatro VMs quedó en estado `VM running` más de 24 horas después de la sesión de laboratorio (verificable por el docente en la suscripción) | -5 puntos sobre el total |
| Eliminación del grupo de recursos o de componentes de red/datos que la Sesión 5 necesita reutilizar (en lugar de solo desasignar las VMs) | -8 puntos sobre el total — obliga a reconstruir parte de la arquitectura en la Sesión 5 |
| Nombres de recursos que no permiten identificar a qué componente de la arquitectura corresponden | Hasta -3 puntos en la sección D |
| Cambiar de Opción 1 a Opción 2 (o viceversa) sin documentar el motivo | Hasta -2 puntos en la sección B |

---

[← Volver al README](README.md)
