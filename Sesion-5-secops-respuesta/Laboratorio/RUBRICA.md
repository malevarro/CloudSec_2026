[← Volver al README](README.md)

# Rúbrica de evaluación — Laboratorio Sesión 5

**Peso de esta entrega:** 20 % de la nota final del módulo (igual que las Sesiones 1 a 4).
**Formato de entrega:** un único archivo PDF, cargado en Google Classroom.

> ⚠️ **Regla de cumplimiento de formato — no negociable**: cualquier entrega que no sea un único archivo PDF cargado en el canal de Google Classroom asignado a esta sesión se califica automáticamente con **0**, independientemente de la calidad del contenido. Esto incluye: múltiples archivos, enlaces a Google Drive/OneDrive en lugar del archivo adjunto, formatos distintos a PDF (Word, imágenes sueltas, ZIP), y entregas por correo electrónico o cualquier otro canal.

---

## Distribución de puntos (100 puntos = 20 % de la nota final)

| Criterio | Puntos | Qué se evalúa |
|---|---|---|
| 1. Visibilidad configurada | 15 | Evidencia de Log Analytics, Diagnostic Settings y trial de Entra ID P1 correctamente implementados |
| 2. Detección y mapeo MITRE ATT&CK | 25 | Consultas KQL ejecutadas con resultados reales, línea de tiempo reconstruida y mapeo correcto a tácticas/técnicas |
| 3. Contención y erradicación | 20 | Los 4 pasos del playbook ejecutados y verificados con evidencia |
| 4. RCA y lecciones aprendidas | 20 | Cadena de 5 porqués coherente, causa raíz identificada correctamente, recomendaciones concretas y accionables |
| 5. Calidad del informe ejecutivo | 15 | Claridad de redacción, estructura según la plantilla, capturas completas y correctamente etiquetadas |
| 6. Preguntas de repaso | 5 | Las 28 preguntas respondidas con comprensión conceptual (no solo copiadas de la guía) |
| **Total** | **100** | |

---

## Niveles de desempeño por criterio

### 1. Visibilidad configurada (15 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| Excelente | 13-15 | Las 4 fuentes (Activity Log, Key Vault, Flow Logs/Traffic Analytics, Sign-in Logs) están configuradas y con datos verificables en las capturas |
| Bueno | 9-12 | 3 de las 4 fuentes configuradas correctamente |
| Suficiente | 5-8 | 2 de las 4 fuentes configuradas, o todas configuradas pero sin evidencia clara de datos llegando |
| Insuficiente | 0-4 | 1 o ninguna fuente configurada correctamente |

### 2. Detección y mapeo MITRE ATT&CK (25 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| Excelente | 21-25 | Las 3 consultas KQL devuelven resultados reales del propio laboratorio, la línea de tiempo está completa y el mapeo ATT&CK es correcto y bien justificado |
| Bueno | 15-20 | Las consultas funcionan pero con 1-2 imprecisiones en el mapeo ATT&CK o en la línea de tiempo |
| Suficiente | 8-14 | Al menos 1 de las 3 consultas no devuelve resultados propios (se usan datos de ejemplo genéricos) o el mapeo ATT&CK es incorrecto |
| Insuficiente | 0-7 | No hay evidencia de consultas KQL ejecutadas contra datos propios |

### 3. Contención y erradicación (20 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| Excelente | 17-20 | Los 4 pasos del playbook (revocar rol, eliminar SP, rotar secreto, aislar red) ejecutados y verificados con captura de confirmación |
| Bueno | 12-16 | 3 de los 4 pasos completados y verificados |
| Suficiente | 6-11 | 2 de los 4 pasos completados, sin verificación clara |
| Insuficiente | 0-5 | 1 o ningún paso de contención/erradicación evidenciado |

### 4. RCA y lecciones aprendidas (20 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| Excelente | 17-20 | La cadena de 5 porqués llega a una causa raíz de proceso/diseño (no solo técnica), y las recomendaciones son específicas, accionables y conectadas con conceptos de sesiones anteriores |
| Bueno | 12-16 | RCA coherente pero se queda en la causa inmediata; recomendaciones válidas pero genéricas |
| Suficiente | 6-11 | RCA superficial (repite el resumen del incidente sin profundizar); menos de 3 recomendaciones |
| Insuficiente | 0-5 | No hay RCA estructurado o las recomendaciones no se relacionan con el incidente analizado |

### 5. Calidad del informe ejecutivo (15 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| Excelente | 13-15 | Sigue la plantilla completa, redacción clara y ejecutiva, todas las capturas presentes y correctamente etiquetadas en orden |
| Bueno | 9-12 | Sigue la plantilla con 1-2 secciones incompletas o capturas faltantes/mal etiquetadas |
| Suficiente | 5-8 | Estructura desordenada o varias capturas faltantes, pero el contenido técnico es identificable |
| Insuficiente | 0-4 | No sigue la plantilla, o la mayoría de las capturas requeridas no están presentes |

### 6. Preguntas de repaso (5 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| Excelente | 5 | Las 28 preguntas respondidas demostrando comprensión propia, no copia literal de la guía |
| Bueno | 3-4 | La mayoría de las preguntas respondidas correctamente |
| Suficiente | 2 | Menos de la mitad de las preguntas respondidas o respuestas muy superficiales |
| Insuficiente | 0-1 | Preguntas sin responder o respuestas copiadas sin comprensión evidente |

---

## Notas para el estudiante

- Esta rúbrica evalúa **evidencia**, no la simple afirmación de haber hecho algo. Una acción sin su captura correspondiente se considera no realizada para efectos de calificación.
- Los datos de tus consultas KQL y capturas deben corresponder a **tu propio entorno** (tu `appId`, tus IPs, tus horas). Capturas idénticas entre dos entregas se tratan como indicio de fraude académico y se remiten al proceso disciplinario correspondiente.
- Si por alguna razón no lograste completar un paso técnico, documenta en el informe **qué intentaste, qué error obtuviste y qué hipótesis tienes sobre la causa** — esto puede recibir crédito parcial en el criterio correspondiente, mientras que una sección simplemente vacía no lo recibe.

---

[← Volver al README](README.md)
