# Rúbrica de Evaluación — Laboratorio 3: Datos e Identidad y Acceso (IAM)

**Peso:** este laboratorio equivale al **20 % de la nota final del módulo**.
**Escala:** 100 puntos, convertidos proporcionalmente al 20 % de la nota del módulo.

Esta rúbrica es la que el instructor usa para calificar tu entrega. Revísala **antes** de empezar el laboratorio, no solo al terminar — te dice exactamente qué evidencia produce puntos.

---

## 1. Formato de entrega (requisito de admisibilidad — no otorga puntos, pero es obligatorio)

| Requisito | Detalle |
|---|---|
| Formato | **Un único archivo PDF** |
| Plataforma de entrega | **Google Classroom** — tarea de la Sesión 3 |
| Nombre del archivo | `Apellido_Nombre_Sesion3_Laboratorio.pdf` |
| Archivos NO aceptados | `.docx`, `.zip`, `.rar`, enlaces a Google Drive/OneDrive, múltiples archivos, capturas sueltas sin documento que las contenga |

> ❌ **Una entrega que no cumpla el formato anterior recibe 0 puntos**, sin excepción, independientemente de la calidad del trabajo realizado en Azure. Ver el detalle completo en [`06-consolidacion-informe.md`](./06-consolidacion-informe.md).

---

## 2. Distribución de puntos

| # | Sección evaluada | Puntos |
|---|---|---|
| 1 | Entra ID + RBAC | 15 |
| 2 | Cifrado de Storage con Customer‑Managed Key | 15 |
| 3 | Cifrado de disco de VM con Disk Encryption Set | 15 |
| 4 | Integración SAML (Entra ID ↔ sptest.iamshowcase.com) | 15 |
| 5 | Integración OIDC (App Registration ↔ ms-identity-python-webapp) | 15 |
| 6 | Preguntas de repaso (todas las secciones) | 15 |
| 7 | Calidad general del informe | 10 |
| | **Total** | **100** |

---

## 3. Niveles de desempeño por sección (1 a 5)

Cada una de las cinco secciones técnicas (15 puntos cada una) se califica con estos cuatro niveles:

| Nivel | Puntos | Descripción |
|---|---|---|
| **Excelente** | 13–15 | El recurso está correctamente configurado, todas las capturas solicitadas están presentes, son legibles y corresponden exactamente a lo pedido (nombres de recursos visibles, estado correcto). La configuración sigue el principio de mínimo privilegio / buenas prácticas de llaves donde aplica. |
| **Aceptable** | 9–12 | El recurso funciona pero con configuración subóptima (p. ej. rol más amplio del necesario, falta alguna captura secundaria) o hay inconsistencias menores entre lo mostrado y lo esperado. |
| **Insuficiente** | 1–8 | Faltan capturas clave, la configuración no demuestra que el control realmente funciona (p. ej. no se ve el estado "Habilitado" del cifrado), o hay errores conceptuales evidentes. |
| **No presentado** | 0 | La sección no aparece en el informe, o las capturas no corresponden al laboratorio del estudiante (pantallas genéricas, capturas de otro entorno, etc.). |

### Criterios específicos por sección

**1. Entra ID + RBAC (15 pts)**
- Usuario de prueba creado y visible en Entra ID.
- Asignación de rol con alcance limitado al grupo de recursos (no a la suscripción completa).
- Evidencia de verificación (captura del intento de acceso restringido).

**2. Cifrado de Storage con CMK (15 pts)**
- Key Vault con soft‑delete y purge protection habilitados.
- Storage Account con identidad administrada habilitada.
- Cifrado configurado como "Customer-managed keys" y visible como tal en el Portal.
- Permiso correcto otorgado (rol `Key Vault Crypto Service Encryption User`, no un rol más amplio).

**3. Cifrado de disco de VM con DES (15 pts)**
- Disk Encryption Set creado y vinculado a una llave del Key Vault.
- VM de tamaño B1s (Free tier eligible) con el disco configurado con cifrado CMK desde la creación.
- Captura del disco mostrando el Disk Encryption Set asociado.

**4. Integración SAML (15 pts)**
- Enterprise Application configurada con Identifier y Reply URL correctos.
- Usuario de prueba asignado a la aplicación.
- Evidencia de un inicio de sesión SAML exitoso mostrando los claims recibidos en sptest.iamshowcase.com.

**5. Integración OIDC (15 pts)**
- App Registration con Redirect URI correcto (`http://localhost:5000/getAToken`).
- Aplicación ejecutándose localmente sin errores.
- Evidencia de login exitoso mostrando el nombre del usuario autenticado.
- El client secret **no aparece en texto plano** en ninguna captura (ver advertencia de seguridad en la Sección 5).

---

## 4. Preguntas de repaso (15 pts)

- Se evalúan **todas** las preguntas de repaso de las cinco secciones técnicas (no las de la sección 0).
- Cada respuesta debe estar redactada con las propias palabras del estudiante, con base en lo configurado, no copiada de la guía o de un compañero.
- Puntaje proporcional al número de preguntas respondidas correcta y completamente.

---

## 5. Calidad general del informe (10 pts)

| Criterio | Puntos |
|---|---|
| Estructura clara: portada, secciones en el mismo orden de la guía, capturas numeradas | 3 |
| Capturas legibles (resolución suficiente, sin recortes que oculten información relevante) | 3 |
| Redacción clara y propia, sin errores que impidan la comprensión | 2 |
| Evidencia de limpieza de recursos al final (Sección 7 completada) | 2 |

---

## 6. Tabla de penalizaciones

| Situación | Penalización |
|---|---|
| Entrega fuera de plazo (por cada día calendario de retraso) | -10 puntos sobre el total, hasta un máximo de 3 días; después no se recibe |
| Formato de archivo incorrecto | 0 puntos automático (ver sección 1) |
| Capturas de pantalla faltantes en una sección | -3 puntos por captura faltante, dentro del máximo de esa sección |
| Client secret, contraseñas o llaves expuestas en texto plano en una captura | -5 puntos sobre el total (riesgo de seguridad real) |
| Recursos de Azure no eliminados al momento de la entrega (evidenciable por el instructor) | -5 puntos sobre el total |
| Respuestas a preguntas de repaso idénticas entre dos o más estudiantes | 0 puntos en la sección de preguntas para todos los involucrados |

---

## 7. Nota sobre el uso de asistentes de IA

Se permite el uso de asistentes de IA (incluyendo Claude) como apoyo para entender conceptos o depurar errores puntuales durante el laboratorio. **No se permite** que el informe final, y en particular las respuestas a las preguntas de repaso, sean generadas íntegramente por IA sin comprensión del estudiante — el instructor puede solicitar una sustentación oral breve de cualquier entrega.
