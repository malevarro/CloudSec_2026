[⬅ Volver al README](./README.md) · [⬅ Sección anterior](./05-integracion-oidc.md)

# 6. Consolidación y entrega del informe

## 🎯 Objetivo

Armar **un único documento PDF** con toda la evidencia del laboratorio (capturas + respuestas a las preguntas de repaso) y entregarlo correctamente en Google Classroom.

---

## ⚠️ Regla de entrega — léela primero

> **La entrega del laboratorio se realiza únicamente en un único archivo en formato PDF, cargado en Google Classroom.**

- ❌ No se aceptan archivos `.docx`, `.zip`, `.rar`.
- ❌ No se aceptan enlaces a Google Drive, OneDrive, Dropbox ni similares.
- ❌ No se aceptan múltiples archivos (uno por sección, capturas sueltas, etc.).
- ❌ No se acepta un enlace a un repositorio de GitHub como sustituto del PDF.
- ✅ Un (1) solo archivo, extensión `.pdf`, subido directamente al cuadro de entrega de la tarea en Google Classroom.

Una entrega que no cumpla este formato recibe **0 puntos**, sin importar la calidad del trabajo técnico realizado. Ver [`RUBRICA-EVALUACION.md`](./RUBRICA-EVALUACION.md).

---

## Paso 1 — Elige tu herramienta para armar el documento

Cualquiera de estas opciones sirve; usa la que ya conozcas:

| Herramienta | Cómo exportar a PDF |
|---|---|
| Microsoft Word / Google Docs | Pega las capturas y el texto en orden, luego "Archivo > Descargar/Exportar > PDF". |
| Google Slides / PowerPoint | Una diapositiva por captura con su numeración, luego exporta como PDF. |
| Editor de Markdown (si ya usaste Markdown en el curso) | Escribe el informe en `.md` con las imágenes embebidas y conviértelo con una herramienta como Pandoc, o con la exportación a PDF de tu editor. |

---

## Paso 2 — Estructura recomendada del documento

1. **Portada:** nombre completo, curso, "Laboratorio 3 — Datos e Identidad y Acceso (IAM)", fecha.
2. **Sección 0 — Preparación del entorno:** capturas 0.1 a 0.5 + respuestas a las 4 preguntas de repaso.
3. **Sección 1 — Entra ID + RBAC:** capturas 1.1 a 1.5 + respuestas a las 4 preguntas de repaso.
4. **Sección 2 — Cifrado de Storage:** capturas 2.1 a 2.7 + respuestas a las 4 preguntas de repaso.
5. **Sección 3 — Cifrado de VM's:** capturas 3.1 a 3.6 + respuestas a las 4 preguntas de repaso.
6. **Sección 4 — Integración SAML:** capturas 4.1 a 4.7 + respuestas a las 4 preguntas de repaso.
7. **Sección 5 — Integración OIDC:** capturas 5.1 a 5.5 + respuestas a las 4 preguntas de repaso.
8. **Sección 7 — Evidencia de limpieza:** al menos una captura mostrando el grupo de recursos eliminado (ver la siguiente guía).

Cada captura debe llevar su número de referencia (ej. "Captura 2.6") como pie de imagen o título, en el mismo orden en que aparecen en esta guía — así el instructor puede calificar rápidamente contra la rúbrica.

---

## ✅ Checklist completo de capturas requeridas (35 en total)

Márcalas a medida que las vas guardando durante el laboratorio:

**Sección 0 (5):** ☐ 0.1 ☐ 0.2 ☐ 0.3 ☐ 0.4 ☐ 0.5
**Sección 1 (5):** ☐ 1.1 ☐ 1.2 ☐ 1.3 ☐ 1.4 ☐ 1.5
**Sección 2 (7):** ☐ 2.1 ☐ 2.2 ☐ 2.3 ☐ 2.4 ☐ 2.5 ☐ 2.6 ☐ 2.7
**Sección 3 (6):** ☐ 3.1 ☐ 3.2 ☐ 3.3 ☐ 3.4 ☐ 3.5 ☐ 3.6
**Sección 4 (7):** ☐ 4.1 ☐ 4.2 ☐ 4.3 ☐ 4.4 ☐ 4.5 ☐ 4.6 ☐ 4.7
**Sección 5 (5):** ☐ 5.1 ☐ 5.2 ☐ 5.3 ☐ 5.4 ☐ 5.5
**Sección 7 (1 mínimo):** ☐ evidencia de eliminación del grupo de recursos

---

## ✅ Checklist completo de preguntas de repaso (24 en total)

- ☐ 4 preguntas de la Sección 0
- ☐ 4 preguntas de la Sección 1
- ☐ 4 preguntas de la Sección 2
- ☐ 4 preguntas de la Sección 3
- ☐ 4 preguntas de la Sección 4
- ☐ 4 preguntas de la Sección 5

---

## Paso 3 — Nombrar el archivo

Usa exactamente este formato:

```
Apellido_Nombre_Sesion3_Laboratorio.pdf
```

Ejemplo: `VargasRojas_Manuel_Sesion3_Laboratorio.pdf`

---

## Paso 4 — Subir a Google Classroom

1. Ingresa a Google Classroom con tu cuenta institucional.
2. Entra al curso de la especialización y busca la tarea **"Laboratorio 3 — Datos e Identidad y Acceso (IAM)"**.
3. Haz clic en **"Ver tarea"** y luego en **"+ Agregar o crear"** > **"Archivo"**.
4. Selecciona tu PDF ya nombrado según el paso anterior.
5. Haz clic en **"Entregar"**. Confirma que el estado de la tarea cambia a "Entregado".

📸 No es necesario tomar captura de este paso, pero verifica el estado "Entregado" antes de cerrar la sesión.

---

[➡ Continuar con la Sección 7 — Limpieza de recursos](./07-limpieza-recursos.md)
