[⬅ Volver al README](./README.md) · [⬅ Sección anterior](./07-limpieza-recursos.md)

# 8. Solución de problemas comunes

Errores frecuentes organizados por sección. Si tu error no aparece aquí, revisa el mensaje completo que muestra Azure (suele incluir el motivo exacto) antes de buscar ayuda externa.

---

## Sección 0 — Preparación del entorno

| Problema | Causa probable | Solución |
|---|---|---|
| `python --version` no reconoce el comando | No se agregó Python al PATH durante la instalación | Reinstala Python marcando "Add python.exe to PATH", o agrega la ruta manualmente a las variables de entorno. |
| `Install-Module -Name Az` falla con error de permisos | PowerShell no se abrió con permisos suficientes para el scope elegido | Usa `-Scope CurrentUser` (ya incluido en el comando de esta guía) en vez de instalar para todo el sistema. |
| No aparece la opción de crear presupuesto | Tu cuenta no tiene rol de "Cost Management Contributor" o similar | Si usaste tu cuenta Free Trial personal, ya eres owner de la suscripción — verifica que estás en la suscripción correcta en el selector superior del Portal. |

---

## Sección 1 — Entra ID + RBAC

| Problema | Causa probable | Solución |
|---|---|---|
| No puedo iniciar sesión como `lab3.user` | La contraseña temporal expiró o no se cambió correctamente | Ve a Entra ID > Users > `lab3.user` > "Reset password" y genera una nueva. |
| `lab3.user` SÍ puede crear recursos (no debería) | El rol se asignó con alcance en la suscripción, no en el grupo de recursos | Revisa "Access control (IAM)" > "Role assignments", verifica la columna "Scope" y corrige si es necesario. |
| No aparece la opción "Security defaults" | Ya existe una política de Conditional Access en el tenant | Documenta el mensaje que ves y continúa — no es bloqueante para el resto del laboratorio. |

---

## Sección 2 — Cifrado de Storage

| Problema | Causa probable | Solución |
|---|---|---|
| "Key vault name already taken" | Los nombres de Key Vault son únicos en todo Azure, no solo en tu suscripción | Agrega un sufijo numérico a tu alias, ej. `kv-lab3-mvr01x`. |
| No puedo seleccionar "Customer-managed keys" en Encryption | El rol del Paso 5 aún no se propagó | Espera 2-3 minutos y refresca la página; los cambios de RBAC pueden tardar en aplicarse. |
| Error al crear la llave: "Caller is not authorized" | Tu cuenta no tiene el rol `Key Vault Administrator` sobre el vault recién creado | Este rol se asigna automáticamente al creador si elegiste el modelo RBAC en el Paso 1; verifica en "Access control (IAM)" del vault que tu cuenta aparezca con ese rol. |

---

## Sección 3 — Cifrado de VM's

| Problema | Causa probable | Solución |
|---|---|---|
| No aparece "Standard_B1s" en la lista de tamaños | La región elegida no tiene capacidad disponible para ese tamaño en este momento | Cambia de región (ej. de "East US" a "West US 2" o "West Europe") y vuelve a intentar. |
| No aparece la etiqueta "Free tier eligible" | Estás usando una imagen o tamaño distinto al indicado | Vuelve a la pestaña "Basics" y confirma que elegiste exactamente `Ubuntu Server 22.04 LTS` y `Standard_B1s`. |
| Falla la creación de la VM por permisos sobre el Disk Encryption Set | El DES no tiene el rol `Key Vault Crypto Service Encryption User` sobre el Key Vault | Repite el Paso 3 de la Sección 3 (otorgar acceso manualmente). |

---

## Sección 4 — Integración SAML

| Problema | Causa probable | Solución |
|---|---|---|
| Error "AADSTS750054" o similar sobre Reply URL | El Reply URL configurado en Entra ID no coincide exactamente con el que espera sptest.iamshowcase.com | Vuelve a copiar el valor exacto desde el sitio (Paso 1) y compáralo carácter por carácter con lo guardado en el Paso 3. |
| sptest.iamshowcase.com no reconoce el certificado | Se pegó el contenido incorrecto (por ejemplo, con saltos de línea alterados) | Vuelve a descargar el certificado Base64 y ábrelo con un editor de texto plano antes de copiar. |
| `lab3.user` no puede iniciar sesión en la app | No fue asignado en "Users and groups" de la Enterprise Application | Repite el Paso 5 de la Sección 4. |

---

## Sección 5 — Integración OIDC

| Problema | Causa probable | Solución |
|---|---|---|
| Error "AADSTS50011: redirect URI mismatch" | El Redirect URI en Entra ID no coincide exactamente con el configurado en `app_config.py` | Ambos deben ser idénticos carácter por carácter: `http://localhost:5000/getAToken`. |
| `pip install -r requirements.txt` falla | El entorno virtual no está activado, o la versión de Python es muy antigua | Verifica que veas `(venv)` en tu terminal y que `python --version` sea 3.10 o superior. |
| La app arranca pero el login falla con error de "invalid_client" | El client secret expiró, se copió mal, o se generó uno nuevo sin actualizar `app_config.py` | Genera un nuevo client secret (Sección 5, Paso 3) y actualiza el archivo de configuración inmediatamente. |
| Puerto 5000 ya está en uso | Otra aplicación (a veces AirPlay en macOS) está usando ese puerto | Cierra la otra aplicación, o cambia el puerto tanto en el Redirect URI de Entra ID como en la configuración del proyecto — deben coincidir siempre. |

---

## Sección 7 — Limpieza

| Problema | Causa probable | Solución |
|---|---|---|
| La eliminación del grupo de recursos tarda mucho o falla | El Key Vault con purge protection puede retrasar el proceso | Espera; si falla, reintenta la eliminación — es normal que tome varios minutos. |
| Quiero reutilizar el mismo nombre de Key Vault y Azure dice que ya existe | El vault anterior sigue "soft-deleted" | Ve a "Key Vaults" > "Manage deleted vaults" y púrgalo manualmente, o usa un nombre distinto. |

---

## ¿Sigues con problemas?

1. Lee el mensaje de error completo — Azure casi siempre indica la causa exacta al final del mensaje.
2. Revisa que estás en la suscripción y el grupo de recursos correctos (selector superior del Portal).
3. Consulta con tus compañeros o el instructor durante el horario de laboratorio, citando el mensaje de error exacto y la sección en la que ocurrió.

---

[⬅ Volver al README](./README.md)
