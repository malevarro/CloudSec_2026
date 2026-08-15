[🏠 README — Laboratorio 2](README.md) · Sección 8 de 8

## Solución de problemas frecuentes

| Problema | Causa probable | Solución |
| --- | --- | --- |
| `az` no se reconoce como comando | Azure CLI no está en el PATH o no se reinició la terminal | Cierra y abre una nueva ventana de PowerShell; reinstala si persiste |
| `prowler` no se reconoce como comando | La carpeta `Scripts` de Python no está en el PATH | Ver nota en la sección [4.1](#41-instalar-prowler) |
| Error `AuthorizationFailed` al ejecutar Prowler/CloudSploit | El rol Reader no se asignó correctamente o aún no se propagó | Espera 2-3 minutos tras asignar el rol; verifica en IAM → Role assignments que el Service Principal aparece |
| El nombre del Storage Account ya existe | Los nombres son únicos globalmente en todo Azure, no solo en tu cuenta | Agrega números o iniciales adicionales al nombre |
| `az stack group create` falla con error de permisos | Tu cuenta no tiene permisos suficientes en el grupo de recursos | Verifica que tu cuenta tiene el rol Owner o Contributor sobre `rg-lab2-jvargas` |
| El Secure Score no cambia después de remediar | Defender for Cloud tarda horas en re-evaluar | Es esperado; documenta la recomendación como "remediada" con la captura del cambio de configuración, no esperes el número actualizado en tiempo real |
| CloudSploit falla con `Cannot find module` | `npm install` no terminó correctamente | Borra la carpeta `node_modules` y ejecuta `npm install` de nuevo |

---

⬅️ Anterior: [Parte 7 · Limpieza de recursos (obligatorio en Free Tier)](07-limpieza-recursos.md) · 🏠 [README](README.md) · Siguiente ➡️: [Volver al inicio](README.md)
