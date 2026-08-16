[⬅ Volver al README](./README.md) · [⬅ Sección anterior](./06-consolidacion-informe.md)

# 7. Limpieza de recursos (obligatorio)

## 🎯 Objetivo

Eliminar **todos** los recursos creados durante el laboratorio, tanto los que están dentro del grupo de recursos como los objetos de Entra ID que viven fuera de él. Esta sección **no es opcional**: dejar recursos activos (especialmente la VM y su disco) puede generar cargos con el tiempo, y la rúbrica penaliza recursos no eliminados.

---

## Paso 1 — Detener y eliminar la máquina virtual

Aunque vamos a eliminar todo el grupo de recursos en el Paso 3 (lo cual es suficiente), es buena práctica detener explícitamente la VM primero:

1. Ve a tu VM `vm-lab3-<alias>` en el Portal.
2. Haz clic en **"Stop"** en la barra superior y espera a que el estado cambie a "Stopped (deallocated)".

📸 **Captura para el informe — 7.1:** la VM en estado "Stopped (deallocated)".

---

## Paso 2 — Eliminar los objetos de Entra ID (fuera del grupo de recursos)

Estos recursos **no** se eliminan al borrar el grupo de recursos, porque no pertenecen a él — hay que eliminarlos manualmente.

1. **Enterprise Application SAML:** Entra ID > "Enterprise applications" > `Lab3-SAML-iamshowcase` > "Properties" > **"Delete"**.
2. **App Registration OIDC:** Entra ID > "App registrations" > `Lab3-OIDC-PythonWebApp` > **"Delete"** (en la barra superior de la pantalla "Overview").
3. **Usuario de prueba:** Entra ID > "Users" > `lab3.user` > **"Delete"**.

> Si quieres conservar `lab3.user` para practicar por tu cuenta más adelante, puedes dejarlo — no genera ningún costo, solo revisa que no tenga asignaciones de rol innecesarias activas.

---

## Paso 3 — Eliminar el grupo de recursos completo

Esta es la forma más simple y segura de eliminar **todo lo demás de una sola vez**: Key Vault, Storage Account, Disk Encryption Set, VM, disco, red virtual y cualquier otro recurso asociado.

1. Ve a **"Resource groups"** y entra a `rg-lab3-<alias>`.
2. Haz clic en **"Delete resource group"** en la barra superior.
3. Azure te pedirá escribir el nombre del grupo de recursos para confirmar — escríbelo exactamente y haz clic en **"Delete"**.
4. Espera a que el proceso termine (puede tardar varios minutos, especialmente por el Key Vault).

📸 **Captura para el informe — 7.2:** la pantalla de confirmación de eliminación, o el mensaje de Azure confirmando que el grupo de recursos ya no existe (búscalo de nuevo en "Resource groups" y confirma que no aparece en la lista).

> ⚠️ **Nota sobre el Key Vault:** por tener "Purge protection" habilitado (Sección 2), el Key Vault no se borra por completo de inmediato — queda en estado **"soft-deleted"** durante un período de retención (90 días por defecto). Esto **no genera ningún costo** mientras está en ese estado, y no afecta tu cuota de recursos activos. Si necesitas reutilizar exactamente el mismo nombre de Key Vault antes de que termine ese período, tendrías que purgarlo manualmente desde "Key Vaults" > pestaña "Manage deleted vaults" — no es necesario para este laboratorio.

---

## Paso 4 — Verificar en Cost Management

1. Ve a **"Cost Management + Billing"** > **"Cost analysis"**.
2. Revisa que no haya cargos inesperados acumulándose después de la eliminación.
3. Confirma que la alerta de presupuesto configurada en la Sección 0 sigue activa para futuros laboratorios.

---

## ❓ Preguntas de repaso — Sección 7

1. ¿Por qué eliminar el grupo de recursos no elimina automáticamente el usuario `lab3.user` ni las aplicaciones de Entra ID?
2. ¿Qué implica que el Key Vault quede "soft-deleted" en vez de eliminarse por completo de inmediato?

---

[➡ Si algo no funcionó como se esperaba, revisa la Sección 8 — Solución de problemas](./08-solucion-problemas.md)
