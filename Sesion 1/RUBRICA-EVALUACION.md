[⬅ Volver al índice](README.md)

# 📋 Rúbrica de Evaluación — Laboratorio Sesión 1

**Especialización en Ciberseguridad · Seguridad en la Nube**
**Peso en la nota del módulo:** 20 %
**Puntaje total de esta rúbrica:** 100 puntos (equivalentes al 20 % de la nota)

---

## 🚨 Instrucciones de entrega — LEER ANTES DE EMPEZAR

> ## ⚠️ La entrega se recibe **ÚNICAMENTE** en formato **PDF**, cargado en la plataforma de **Google Classroom** dispuesta para este laboratorio.
>
> ### ❌ NO se reciben, bajo ninguna circunstancia, entregas en otro formato:
> - ❌ Archivos `.docx`, `.doc` (Word)
> - ❌ Archivos `.pptx`, `.ppt` (PowerPoint)
> - ❌ Archivos `.zip`, `.rar` u otros comprimidos
> - ❌ Archivos `.json` sueltos (el modelo de Threat Dragon debe ir **incrustado como capturas** dentro del PDF, no como archivo adjunto)
> - ❌ Enlaces a Google Docs, Notion, OneDrive u otras plataformas externas en lugar del archivo
> - ❌ Capturas de pantalla sueltas (`.png`, `.jpg`) fuera de un PDF
> - ❌ Entregas por correo electrónico, WhatsApp, o cualquier canal distinto a Google Classroom
>
> **Una entrega en un formato no permitido se considera NO ENTREGADA** y se calificará con **0 puntos**, sin importar la calidad del trabajo realizado. Es responsabilidad del estudiante exportar y verificar su documento como PDF antes de cargarlo.
>
> 💡 **Cómo generar el PDF:** si trabajaste tus evidencias en Word, Google Docs o cualquier editor, usa la opción **Archivo → Descargar/Exportar → PDF (.pdf)** antes de subir el archivo a Google Classroom.

---

## 📎 Estructura mínima que debe contener el PDF entregado

El documento PDF debe incluir, en este orden, la evidencia de las 6 secciones del laboratorio:

1. **Portada:** nombre completo, fecha, nombre del laboratorio ("Laboratorio Sesión 1 — Fundamentos de Cloud Computing").
2. **Sección 1 — Threat Modeling:** capturas del diagrama DFD completo y de la lista de las 6 amenazas STRIDE con su severidad.
3. **Sección 2 — Alta en Azure Free Tier:** captura de la suscripción activa y del presupuesto (`presupuesto-lab1`) configurado con sus alertas.
4. **Sección 3 — Exploración del Portal:** captura del menú de Favoritos configurado.
5. **Sección 4 — ARM / Resource Groups:** captura del Resource Group creado, con sus etiquetas (`tags`) visibles.
6. **Sección 5 — Mapeo de responsabilidades:** tabla de responsabilidades completa (VM / App Service / SaaS) con las 8 filas llenas.
7. **Sección 6 — Managed Identity:** capturas de (a) Identity = On con el Object ID, (b) los dos Role assignments (Storage y Key Vault), (c) las respuestas JSON del `curl` al IMDS mostrando el `access_token`, y (d) el Resource Group eliminado al finalizar.
8. **Respuestas a las preguntas de repaso** de cada una de las 6 secciones de la guía.

> 💡 Todas las capturas marcadas con el ícono 📸 a lo largo de la guía de laboratorio son las que debes incluir en tu PDF.

---

## 🧮 Rúbrica de calificación por sección

Cada sección se califica en 4 niveles de desempeño. El puntaje obtenido en cada sección se suma para obtener el total sobre 100 puntos.

### Sección 1 — Threat Modeling con OWASP Threat Dragon (20 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| **Excelente** | 18–20 | El DFD incluye correctamente el actor, el proceso, los 2 almacenes de datos y la frontera de confianza. Las 6 amenazas STRIDE están registradas, correctamente clasificadas por tipo, con severidad coherente y mitigaciones concretas y pertinentes (no genéricas). |
| **Bueno** | 13–17 | El DFD está completo pero con imprecisiones menores (ej. nombres poco descriptivos). Las 6 amenazas están presentes pero 1 o 2 tienen tipo STRIDE incorrecto o mitigación vaga. |
| **Aceptable** | 7–12 | El DFD está incompleto (falta algún elemento o la frontera de confianza) o solo se registraron entre 3 y 5 amenazas STRIDE. |
| **Insuficiente** | 0–6 | No hay diagrama, o hay menos de 3 amenazas registradas, o las amenazas no corresponden a la metodología STRIDE. |

### Sección 2 — Alta en Azure Free Tier (10 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| **Excelente** | 9–10 | Suscripción Free Trial activa evidenciada. Presupuesto de USD 10 creado con al menos 2 alertas (50 % y 90 %) configuradas y visibles en la captura. |
| **Bueno** | 6–8 | Suscripción activa evidenciada, presupuesto creado pero con solo 1 alerta configurada o con un monto distinto al indicado sin justificación. |
| **Aceptable** | 3–5 | Suscripción activa evidenciada, pero no se configuró presupuesto/alerta de gasto. |
| **Insuficiente** | 0–2 | No hay evidencia de la suscripción activa. |

### Sección 3 — Exploración del Portal (5 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| **Excelente** | 5 | Captura del menú de Favoritos con los 6 servicios solicitados correctamente marcados. |
| **Bueno** | 3–4 | Favoritos configurados parcialmente (entre 4 y 5 de los 6 servicios). |
| **Aceptable** | 1–2 | Menos de 4 servicios marcados como favoritos, o evidencia poco clara. |
| **Insuficiente** | 0 | No hay evidencia de esta sección. |

### Sección 4 — Azure Resource Manager / Resource Groups (10 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| **Excelente** | 9–10 | Resource Group creado con el nombre según la convención indicada, en una región válida, con las 2 etiquetas (`curso`, `sesion`) correctamente aplicadas y visibles. |
| **Bueno** | 6–8 | Resource Group creado correctamente pero le falta una etiqueta o el nombre no sigue exactamente la convención solicitada. |
| **Aceptable** | 3–5 | Resource Group creado pero sin etiquetas, o con nombre genérico no personalizado. |
| **Insuficiente** | 0–2 | No hay evidencia de un Resource Group creado. |

### Sección 5 — Mapeo de responsabilidades por servicio (15 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| **Excelente** | 13–15 | Tabla de 8 filas completamente diligenciada y correcta (coincide con el modelo de responsabilidad compartida visto en la teoría) para los 3 modelos de servicio. Incluye la reflexión de conexión con el modelo de amenazas de la Sección 1. |
| **Bueno** | 9–12 | Tabla completa con 1 o 2 errores conceptuales en la asignación de responsabilidades. |
| **Aceptable** | 5–8 | Tabla incompleta (faltan filas) o con más de 2 errores conceptuales. |
| **Insuficiente** | 0–4 | Tabla no entregada o con la mayoría de las celdas vacías o incorrectas. |

### Sección 6 — Managed Identity: App Service → Storage + Key Vault (30 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| **Excelente** | 27–30 | Evidencia completa de: App Service en plan F1, Managed Identity habilitada (Object ID visible), roles RBAC mínimos asignados correctamente en Storage **y** Key Vault (no roles excesivos como `Contributor`/`Owner`), tokens obtenidos exitosamente desde el IMDS para ambos servicios, ausencia de credenciales confirmada en Application settings, y Resource Group eliminado al finalizar. |
| **Bueno** | 20–26 | Se completó la integración con **uno solo** de los dos servicios (Storage **o** Key Vault) con evidencia completa, o falta 1 de los checkpoints menores (ej. no se muestra la limpieza final). |
| **Aceptable** | 12–19 | La Managed Identity fue habilitada pero la obtención del token falla o no se evidencia, o los roles asignados no siguen el principio de mínimo privilegio (ej. se usó `Owner`). |
| **Insuficiente** | 0–11 | No hay evidencia de Managed Identity funcional, o se usaron credenciales tradicionales (connection strings/claves) en lugar de identidad administrada. |

### Preguntas de repaso y calidad general del documento (10 pts)

| Nivel | Puntos | Descripción |
|---|---|---|
| **Excelente** | 9–10 | Se respondieron todas las preguntas de repaso de las 6 secciones, con argumentos propios que conectan correctamente los conceptos teóricos con la práctica realizada. El documento está ordenado, con portada y secciones claramente identificadas. |
| **Bueno** | 6–8 | Se respondió la mayoría de las preguntas (más del 70 %), con conexión teórica adecuada aunque superficial en algunas respuestas. |
| **Aceptable** | 3–5 | Se respondió menos del 70 % de las preguntas, o las respuestas son copias literales de la guía sin elaboración propia. |
| **Insuficiente** | 0–2 | No se incluyeron respuestas a las preguntas de repaso. |

---

## 🧾 Tabla resumen para el calificador

| Sección | Puntaje máximo | Puntaje obtenido |
|---|---|---|
| 1. Threat Modeling (OWASP Threat Dragon) | 20 | |
| 2. Alta en Azure Free Tier | 10 | |
| 3. Exploración del Portal | 5 | |
| 4. ARM / Resource Groups | 10 | |
| 5. Mapeo de responsabilidades por servicio | 15 | |
| 6. Managed Identity (Storage + Key Vault) | 30 | |
| Preguntas de repaso y calidad del documento | 10 | |
| **TOTAL** | **100** | |

**Nota final del laboratorio** = (Puntaje obtenido ÷ 100) × 20 % de la nota del módulo.

---

## ⛔ Penalizaciones adicionales

| Situación | Penalización |
|---|---|
| Entrega en un formato distinto a PDF | **0 puntos** (no se evalúa el contenido) |
| Entrega fuera de la plataforma Google Classroom (correo, chat, enlace externo, etc.) | **0 puntos** (se considera no entregada) |
| Entrega tardía (después de la fecha y hora límite fijada en Google Classroom) | Según política general del curso indicada en el syllabus — contactar al instructor **antes** de la fecha límite si existe una causa justificada |
| Evidencia de recursos creados en un nivel de precio distinto al gratuito (`F1`, `Standard LRS`) sin justificación | -5 puntos sobre el total, independiente del resto de la evaluación |
| Capturas de pantalla ilegibles, recortadas o que no permiten verificar el paso solicitado | Se califica como si el paso no tuviera evidencia |
| Indicios de que el Resource Group **no fue eliminado** al finalizar (buena práctica de higiene de laboratorio) | -3 puntos sobre el total |

---

## 🧠 Recomendación final para el estudiante

Antes de subir tu PDF a Google Classroom, verifica:

- [ ] El archivo tiene extensión `.pdf` (ábrelo y confirma que se ve correctamente antes de subirlo)
- [ ] Incluye las 8 partes de la estructura mínima indicada arriba
- [ ] Todas las capturas son legibles (texto y nombres de recursos visibles, no borrosas ni recortadas)
- [ ] Respondiste las preguntas de repaso de las 6 secciones
- [ ] Confirmaste, en la última captura, que el Resource Group fue eliminado
- [ ] El archivo se sube en la tarea correspondiente dentro de **Google Classroom**, no por ningún otro medio

---

[⬅ Volver al índice principal](README.md)
