[🏠 README — Laboratorio 2](README.md) · Sección 7 de 8

## Parte 7 — Limpieza de recursos (obligatorio en Free Tier)

Ejecuta esto al terminar, en el mismo orden, para evitar cargos y dejar tu tenant limpio.

```powershell
# 1. Eliminar la asignación de la Initiative de Policy
az policy assignment delete --name "asg-lab2-baseline" --scope "/subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RG_NAME"

# 2. Eliminar el Deployment Stack (y los recursos que administra: VNet + Storage Account)
az stack group delete `
  --name "stack-lab2-jvargas" `
  --resource-group $RG_NAME `
  --action-on-unmanage "deleteResources" `
  --yes

# 3. Eliminar el Template Spec
az ts delete --name "ts-lab2-landingzone" --resource-group $RG_NAME --yes

# 4. Eliminar el grupo de recursos completo (por si quedó algo suelto, como el Storage Account de la Parte 1.7)
az group delete --name $RG_NAME --yes --no-wait

# 5. Eliminar las definiciones de Policy e Initiative personalizadas (a nivel de suscripción)
az policy definition delete --name "lab2-audit-tag-ambiente"
az policy set-definition delete --name "ini-lab2-jvargas-baseline"
```

Finalmente, elimina el/los Service Principal(s) creados en Microsoft Entra ID:

1. Ve a **Microsoft Entra ID → App registrations**.
2. Busca `sp-lab2-jvargas-prowler` (y `sp-lab2-jvargas-cloudsploit` si creaste uno separado).
3. Selecciona la aplicación → **Delete** → confirma.

Por último, **desactiva Foundational CSPM** si no vas a seguir usando Defender for Cloud en tu suscripción (opcional, ya que es gratis y no genera cargos — pero mantiene el entorno limpio para futuros laboratorios):

1. Defender for Cloud → **Environment settings** → tu suscripción → apaga el toggle de **Foundational CSPM** → **Save**.

✅ **Checkpoint final:** en el portal, busca "Grupos de recursos" y confirma que `rg-lab2-jvargas` ya no aparece en la lista (puede tardar unos minutos en desaparecer).

> 📸 **Captura para el informe — 7.1:** la lista de "Grupos de recursos" del portal **sin** `rg-lab2-jvargas`, como evidencia de la limpieza completa.

---

## Preguntas de repaso

1. ¿Por qué es importante eliminar el Service Principal en Microsoft Entra ID y no solo los recursos de Azure?
2. Si olvidas ejecutar esta limpieza, ¿qué recursos de este laboratorio podrían generar algún tipo de costo o riesgo de seguridad si quedan activos indefinidamente?
3. ¿Por qué el orden de los comandos de limpieza importa (por ejemplo, eliminar el Deployment Stack antes que el grupo de recursos)?

---

⬅️ Anterior: [Parte 6 · Consolidación: informe de gobernanza y riesgo](06-consolidacion-informe.md) · 🏠 [README](README.md) · Siguiente ➡️: [Solución de problemas frecuentes](08-solucion-problemas.md)
