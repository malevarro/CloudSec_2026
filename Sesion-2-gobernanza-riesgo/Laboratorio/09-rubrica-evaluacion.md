[🏠 README — Laboratorio 2](README.md) · Sección 9 de 9

# Rúbrica de evaluación — Laboratorio 2: Gobernanza, Riesgo y Postura de Seguridad en Azure

**Seguridad en la Nube · Sesión 2 · Gobernanza, Riesgo, Cumplimiento y Arquitectura Segura**

> Esta rúbrica es pública y se entrega a los estudiantes junto con la guía del laboratorio, para que sepan exactamente cómo se calificará su trabajo **antes** de empezar a desarrollarlo.

---

## ⚠️ Formato de entrega — leer antes de calificar y antes de entregar

> ### 🛑 La entrega se realiza ÚNICAMENTE en un documento PDF, cargado en la plataforma de Google Classroom dispuesta para ello.
>
> - **No se reciben** archivos `.docx`, `.doc`, `.zip`, `.rar`, `.pptx`, `.txt`, `.md`, imágenes sueltas, ni enlaces a Google Drive, OneDrive o cualquier otro repositorio externo.
> - **No se reciben** múltiples archivos por separado (por ejemplo, el reporte HTML de Prowler o el CSV de CloudSploit como adjuntos independientes). Todo el contenido, incluidas las capturas de pantalla, debe estar **dentro de un único PDF**.
> - **No se reciben** entregas por correo electrónico, WhatsApp, ni ningún canal distinto a Google Classroom.
> - Cualquier entrega que no cumpla este formato **se califica con 0.0 (cero)**, sin excepción, independientemente de la calidad del contenido.
> - Es responsabilidad del estudiante verificar, antes de hacer clic en "Entregar" en Classroom, que el archivo adjunto es un único PDF legible y completo.

Ver el detalle de cómo armar y nombrar el archivo en [Parte 6 — Entregable final](06-consolidacion-informe.md#entregable-final).

---

## Resumen de la rúbrica

| Criterio | Peso | Puntos máximos |
| --- | --- | --- |
| 1. Ejecución técnica evidenciada (capturas de las 5 partes) | 40 % | 40 pts |
| 2. Tabla de hallazgos priorizados y mapeo a CCM | 15 % | 15 pts |
| 3. Preguntas de repaso (32 en total) | 20 % | 20 pts |
| 4. Reflexión final | 10 % | 10 pts |
| 5. Calidad, organización y seguridad del documento | 10 % | 10 pts |
| 6. Limpieza de recursos evidenciada | 5 % | 5 pts |
| **Total** | **100 %** | **100 pts** |

> Esta calificación corresponde al **20 % de la nota final del módulo**, según el syllabus de la Sesión 2.

---

## Criterio 1 — Ejecución técnica evidenciada (40 pts)

Se evalúa mediante las **28 capturas de pantalla** (27 obligatorias + 1 opcional) solicitadas a lo largo de la guía (recuadros 📸), verificando que cada parte del laboratorio se ejecutó correctamente.

| Subcriterio | Capturas de referencia | Puntos |
| --- | --- | --- |
| 1.1 Azure Policy e Initiative | 1.2 · 1.3 · 1.4 · 1.5 · 1.6 · 1.7 | 8 pts |
| 1.2 Landing Zone (Deployment Stacks) | 2.5 · 2.6 · 2.7 · 2.8 | 8 pts |
| 1.3 Defender for Cloud (Secure Score) | 3.2 · 3.3 · 3.4 · 3.5 | 6 pts |
| 1.4 CSPM con Prowler | 4.2 · 4.3 · 4.5 · 4.6 (4.7 opcional, +0.5 pt extra) | 9 pts |
| 1.5 CSPM con CloudSploit | 5.1 · 5.3 · 5.4 · 5.5 | 9 pts |

| Nivel | Descripción | Rango de puntos |
| --- | --- | --- |
| **Excelente** | Las capturas de la subsección están todas presentes, en orden, correctamente etiquetadas y muestran resultados exitosos coherentes con los comandos ejecutados. | 90-100 % del puntaje del subcriterio |
| **Bueno** | Faltan 1 captura o hay inconsistencias menores (por ejemplo, etiquetado incompleto) pero la evidencia general es clara. | 70-89 % |
| **Aceptable** | Faltan 2 o más capturas, o las capturas presentes no permiten verificar con claridad que el paso se ejecutó correctamente. | 40-69 % |
| **Insuficiente** | La mayoría de las capturas de la subsección no están presentes, o evidencian que el paso no se completó (errores no resueltos, pantallas vacías). | 0-39 % |

---

## Criterio 2 — Tabla de hallazgos priorizados y mapeo a CCM (15 pts)

| Nivel | Descripción | Puntos |
| --- | --- | --- |
| **Excelente** | La tabla incluye hallazgos de las tres fuentes (Defender, Prowler, CloudSploit), con severidad correctamente asignada, mapeo preciso a un control CCM real, y tratamiento de riesgo (mitigar/transferir/aceptar/evitar) justificado y coherente con la severidad. | 13-15 |
| **Bueno** | La tabla está completa pero el mapeo a CCM es genérico o el tratamiento de riesgo no siempre es coherente con la severidad reportada. | 10-12 |
| **Aceptable** | La tabla incluye hallazgos de solo una o dos herramientas, o el mapeo a CCM está ausente o es incorrecto en varios casos. | 6-9 |
| **Insuficiente** | No hay tabla de hallazgos, o está gravemente incompleta (menos de 3 hallazgos documentados). | 0-5 |

---

## Criterio 3 — Preguntas de repaso (20 pts)

Se evalúan las **32 preguntas de repaso** distribuidas en las Partes 0, 1, 2, 3, 4, 5 y 7 (4+5+5+5+5+5+3). Cada pregunta vale aproximadamente 0.625 pts, agrupadas para calificación práctica en los siguientes bloques:

| Bloque de preguntas | Preguntas incluidas | Puntos |
| --- | --- | --- |
| Parte 0 — Preparación del entorno | 4 preguntas | 2.5 pts |
| Parte 1 — Azure Policy e Initiative | 5 preguntas | 3.5 pts |
| Parte 2 — Landing Zone | 5 preguntas | 3.5 pts |
| Parte 3 — Defender for Cloud | 5 preguntas | 3.5 pts |
| Parte 4 — CSPM con Prowler | 5 preguntas | 3.5 pts |
| Parte 5 — CSPM con CloudSploit | 5 preguntas | 3.5 pts |
| Parte 7 — Limpieza de recursos | 3 preguntas | 2.0 pts |

| Nivel | Descripción por pregunta | Multiplicador |
| --- | --- | --- |
| **Excelente** | Respuesta correcta, completa, en palabras propias, y conecta explícitamente el paso técnico con el concepto teórico de la Sesión 2 (gobernanza, riesgo, CSA, Security by Design/Default, Control/Data Plane). | 100 % |
| **Bueno** | Respuesta correcta pero superficial, o describe el paso técnico sin conectar con la teoría. | 70 % |
| **Aceptable** | Respuesta parcialmente correcta o incompleta. | 40 % |
| **Insuficiente / No responde** | Respuesta ausente, copiada textualmente de la guía sin elaboración propia, o incorrecta. | 0 % |

> **Nota sobre integridad académica:** respuestas idénticas palabra por palabra entre distintos estudiantes, o copiadas directamente de la guía sin elaboración propia, se califican como Insuficiente para esa pregunta en todos los casos involucrados.

---

## Criterio 4 — Reflexión final (10 pts)

| Nivel | Descripción | Puntos |
| --- | --- | --- |
| **Excelente** | Cumple el mínimo de 150 palabras, responde las tres preguntas guía (hallazgo que sorprendió, efecto de Security by Default, priorización según matriz de riesgo 5×5) con argumentos propios y específicos del entorno que auditó. | 9-10 |
| **Bueno** | Cumple el mínimo de palabras y responde las tres preguntas, pero con argumentos genéricos o poco específicos a su propia ejecución. | 6-8 |
| **Aceptable** | No cumple el mínimo de palabras o deja alguna de las tres preguntas sin responder claramente. | 3-5 |
| **Insuficiente** | Reflexión ausente o claramente insuficiente (menos de 50 palabras, o respuesta genérica no relacionada con el laboratorio). | 0-2 |

---

## Criterio 5 — Calidad, organización y seguridad del documento (10 pts)

| Nivel | Descripción | Puntos |
| --- | --- | --- |
| **Excelente** | El PDF sigue el orden exacto solicitado (portada → hallazgos → capturas → preguntas → reflexión), todas las capturas son legibles y están correctamente numeradas/etiquetadas, y **ningún secreto o credencial es visible** en ninguna captura. | 9-10 |
| **Bueno** | El orden general se respeta con alguna desviación menor, o 1-2 capturas tienen baja resolución/recorte deficiente, pero no hay exposición de secretos. | 6-8 |
| **Aceptable** | El documento está desordenado o le faltan etiquetas de identificación en varias capturas, dificultando la revisión. | 3-5 |
| **Insuficiente** | El documento es difícil de seguir, o **se expone un secreto/credencial** (`AZURE_CLIENT_SECRET`, contenido de `azure.json`, contraseñas) en alguna captura — este último caso además debe reportarse al estudiante para que rote la credencial inmediatamente. | 0-2 |

---

## Criterio 6 — Limpieza de recursos evidenciada (5 pts)

| Nivel | Descripción | Puntos |
| --- | --- | --- |
| **Excelente** | Incluye la captura 7.1 mostrando que `rg-lab2-*` ya no existe, y las respuestas de la Parte 7 reflejan comprensión de por qué la limpieza es necesaria en una cuenta Free Tier. | 5 |
| **Aceptable** | Incluye la captura pero las respuestas de la Parte 7 son superficiales. | 2-4 |
| **Insuficiente** | No incluye evidencia de limpieza de recursos. | 0-1 |

---

## Penalizaciones adicionales (aplican sobre el total ya calculado)

| Situación | Penalización |
| --- | --- |
| Formato de entrega distinto a un único PDF en Google Classroom | **Calificación final: 0.0** (ver advertencia al inicio de este documento) |
| Entrega fuera del plazo establecido por el instructor | Según política de entregas tardías del curso (definida en el syllabus) |
| Evidencia de que el laboratorio no se ejecutó en un entorno propio (capturas idénticas entre estudiantes, nombres de recursos ajenos) | Revisión individual por posible falta de integridad académica |
| Exposición de un secreto/credencial en una captura | -2 pts adicionales sobre el Criterio 5, y notificación al estudiante para rotar la credencial |

---

## Hoja de calificación rápida (para uso del evaluador)

| Criterio | Puntos máx. | Puntos obtenidos |
| --- | --- | --- |
| 1. Ejecución técnica evidenciada | 40 | ____ |
| 2. Tabla de hallazgos y mapeo a CCM | 15 | ____ |
| 3. Preguntas de repaso | 20 | ____ |
| 4. Reflexión final | 10 | ____ |
| 5. Calidad, organización y seguridad | 10 | ____ |
| 6. Limpieza de recursos | 5 | ____ |
| Penalizaciones (si aplica) | — | ____ |
| **TOTAL** | **100** | **____** |

---

⬅️ Anterior: [Solución de problemas frecuentes](08-solucion-problemas.md) · 🏠 [README](README.md) · Siguiente ➡️: [Volver al inicio](README.md)
