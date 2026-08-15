[🏠 README — Laboratorio 2](README.md) · Sección 6 de 9

## Parte 6 — Consolidación: informe de gobernanza y riesgo

> ⚠️ **Formato de entrega obligatorio:** este laboratorio se entrega en **un único archivo PDF**, cargado en **Google Classroom**. No se aceptan carpetas comprimidas, múltiples archivos sueltos, ni formatos distintos a PDF. La sección [Entregable final](#entregable-final) de más abajo detalla exactamente cómo armarlo, y la [📊 Rúbrica de evaluación](09-rubrica-evaluacion.md) detalla exactamente cómo se califica.

Con los resultados de las Partes 0 a 5, construye un único documento que se convertirá en tu PDF final. Debe contener, en este orden:

### 1. Portada

Nombre completo, fecha, y el título "Laboratorio 2 — Gobernanza, Riesgo y Postura de Seguridad en Azure".

### 2. Tabla de hallazgos priorizados

Con al menos estas columnas:

| Hallazgo | Herramienta que lo detectó (Defender / Prowler / CloudSploit) | Severidad | Control CCM relacionado (ver Sesión 2, Bloque 3) | Tratamiento propuesto (Mitigar / Transferir / Aceptar / Evitar) |
| --- | --- | --- | --- | --- |

### 3. Todas las capturas de pantalla, en orden

A lo largo de la guía encontrarás recuadros marcados así:

> 📸 **Captura para el informe — N.N:** descripción de qué capturar.

Cada uno de estos recuadros es una captura **obligatoria** para tu PDF. Pégalas **en orden numérico**, cada una con su número de referencia como título (por ejemplo "Captura 1.3"), para que quien revise el laboratorio pueda ubicarlas fácilmente. La checklist completa es:

| Parte | Capturas requeridas |
| --- | --- |
| Parte 0 | 0.1 · 0.2 · 0.5 · 0.6 |
| Parte 1 | 1.2 · 1.3 · 1.4 · 1.5 · 1.6 · 1.7 |
| Parte 2 | 2.5 · 2.6 · 2.7 · 2.8 |
| Parte 3 | 3.2 · 3.3 · 3.4 · 3.5 |
| Parte 4 | 4.2 · 4.3 · 4.5 · 4.6 · 4.7 (opcional) |
| Parte 5 | 5.1 · 5.3 · 5.4 · 5.5 |
| Parte 7 | 7.1 |

**Total: 27 capturas obligatorias + 1 opcional (4.7).**

> 🔒 **Recordatorio de seguridad:** ninguna captura debe mostrar el valor de un secreto (`AZURE_CLIENT_SECRET`), contraseñas, ni el contenido completo de `azure.json`. Si un secreto aparece visible por accidente en una captura, recórtalo o difumínalo antes de incluirla en el PDF, y **rota el secreto** en Microsoft Entra ID por seguridad.

### 4. Todas las respuestas a las preguntas de repaso

Cada sección de la guía (Parte 0 a Parte 7) termina con un bloque **"Preguntas de repaso"**. Responde **todas** las preguntas de **todas** las secciones, organizadas por parte, identificando cada respuesta con el número de la pregunta. En total son:

| Parte | Cantidad de preguntas |
| --- | --- |
| Parte 0 | 4 |
| Parte 1 | 5 |
| Parte 2 | 5 |
| Parte 3 | 5 |
| Parte 4 | 5 |
| Parte 5 | 5 |
| Parte 7 | 3 |

**Total: 32 preguntas de repaso.** Cada respuesta debe tener mínimo 2-3 líneas — se evalúa comprensión, no solo la ejecución de comandos.

### 5. Reflexión final (mínimo 150 palabras)

Responde en un párrafo continuo:

- ¿Qué hallazgo te sorprendió más de todo el laboratorio?
- ¿La configuración "Security by Default" de la plantilla Bicep (Parte 2) evitó algún hallazgo que de otra forma habría aparecido en Defender, Prowler o CloudSploit?
- ¿Qué controles priorizarías primero según la matriz de riesgo 5×5 vista en la Sesión 2?

---

## Entregable final

### Qué se entrega

**Un único archivo PDF** que contenga, en el orden descrito arriba: portada → tabla de hallazgos → las 27-28 capturas de pantalla → las 32 respuestas a preguntas de repaso → reflexión final.

### Cómo armar el PDF

No importa qué herramienta uses para redactar (Word, Google Docs, Markdown exportado, etc.) — lo único que importa es el resultado: **un solo archivo `.pdf`**. Una ruta simple y accesible para todos:

1. Redacta todo el contenido en **Google Docs** (o Word), pegando las capturas donde corresponda según la checklist de la sección anterior.
2. Verifica que el documento sigue el orden: portada → hallazgos → capturas → preguntas de repaso → reflexión.
3. Exporta a PDF: en Google Docs, **Archivo → Descargar → Documento PDF (.pdf)**. En Word, **Archivo → Exportar → Crear PDF/XPS**.
4. Nombra el archivo así (reemplaza con tus datos):

   ```text
   Laboratorio2_GobernanzaRiesgoCSPM_<TusApellidos>_<TuNombre>.pdf
   ```

### Cómo y dónde se entrega

1. Ingresa a **Google Classroom**, en la clase correspondiente a esta sesión.
2. Busca la tarea **"Laboratorio 2 — Gobernanza, Riesgo y CSPM"**.
3. Haz clic en **Agregar o crear → Archivo** y sube tu único PDF.
4. Confirma que Classroom muestra el archivo adjunto **antes** de hacer clic en **Entregar**.
5. Haz clic en **Entregar**.

> ⚠️ **No se aceptan:** enlaces a Google Drive sin permisos de acceso, archivos `.docx`/`.zip`/`.rar`, múltiples archivos separados, ni el reporte HTML de Prowler o el CSV de CloudSploit como archivos independientes — todo ese contenido (como capturas de pantalla) debe estar **dentro** del único PDF.

> ✅ **Antes de subir, verifica:** ¿el PDF abre correctamente en otro dispositivo? ¿están las 27-28 capturas visibles y legibles (no cortadas ni en baja resolución)? ¿respondiste las 32 preguntas de repaso? ¿incluiste la reflexión final?

> 📊 Revisa la **[Rúbrica de evaluación](09-rubrica-evaluacion.md)** antes de entregar — te muestra exactamente cómo se distribuyen los 100 puntos de la calificación.

---

⬅️ Anterior: [Parte 5 · CSPM con CloudSploit](05-cspm-cloudsploit.md) · 🏠 [README](README.md) · Siguiente ➡️: [Parte 7 · Limpieza de recursos (obligatorio en Free Tier)](07-limpieza-recursos.md)
