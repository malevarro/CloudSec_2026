[🏠 README — Laboratorio 2](README.md) · Sección 3 de 8

## Parte 3 — Revisión con Microsoft Defender for Cloud (Secure Score)

### 3.1 Conceptos clave

Microsoft Defender for Cloud tiene dos niveles de CSPM:

| Plan | Costo | Qué incluye | ¿Lo usamos? |
| --- | --- | --- | --- |
| **Foundational CSPM** | **Gratis** | Inventario de activos, evaluación continua contra el Microsoft Cloud Security Benchmark (MCSB), recomendaciones y **Secure Score** | ✅ Sí, es el único plan que activaremos |
| **Defender CSPM** (avanzado) | De pago (por recurso protegido) | Análisis de rutas de ataque, Cloud Security Explorer, escaneo de vulnerabilidades sin agente | ❌ **No lo actives** — tiene costo |

> ⚠️ **Cambio importante a partir del 27 de octubre de 2026:** Microsoft anunció que, desde esa fecha, el plan Foundational CSPM dejará de habilitarse automáticamente en suscripciones **nuevas** — pasará a un modelo "opt-in" (hay que activarlo manualmente). Si tu suscripción ya lo tiene activo, seguirá funcionando sin cambios. Esta guía incluye el paso para activarlo manualmente por si tu entorno ya requiere el opt-in. Fuente: [Opt in to Foundational CSPM — Microsoft Learn](https://learn.microsoft.com/en-us/azure/defender-for-cloud/foundational-cspm-opt-in).

### 3.2 Verificar / habilitar Foundational CSPM (gratis)

1. En el portal, busca **"Microsoft Defender for Cloud"** y entra al servicio.
2. En el menú izquierdo, ve a **Environment settings**.
3. Selecciona tu suscripción Free Tier.
4. En la lista de **Defender plans**, busca la fila **"Foundational CSPM"** (o "CSPM" en versiones anteriores del portal).
5. Confirma que el interruptor (**toggle**) está en **On**. Si está apagado, actívalo.
6. **Verifica cuidadosamente** que ningún otro plan (Defender CSPM, Defender for Servers, Defender for Storage, etc.) esté en **On** — todos esos son de pago. Si alguno aparece activo por defecto, apágalo.
7. Haz clic en **Save**.

**Equivalente en Azure CLI** (opcional):

```powershell
az security pricing create --name "CloudPosture" --tier "Free"
```

<p align="center"><em>[Captura sugerida: pantalla de Environment settings mostrando solo "Foundational CSPM" en On]</em></p>

### 3.3 Explorar el Secure Score

1. En el menú izquierdo de Defender for Cloud, ve a **Overview** (o **Secure Score** directamente).
2. Observa el número principal (0-100 %) — representa qué proporción de las recomendaciones aplicables ya cumples.
3. Haz clic sobre el score para ver el desglose por **grupos de control** (por ejemplo: "Enable MFA", "Secure management ports", "Encrypt data in transit").

> ⏱️ **Nota de tiempos:** si acabas de crear los recursos en la Parte 2, Defender for Cloud puede tardar **varias horas** en completar su primera evaluación completa. Es normal ver el score parcial o "Not yet assessed" en algunos controles durante el laboratorio — lo importante es entender la mecánica, no esperar el número final.

### 3.4 Revisar recomendaciones y remediar al menos dos

1. Ve a **Recommendations** en el menú izquierdo.
2. Filtra por **Resource type = Storage account** para ver las recomendaciones sobre el Storage Account que creaste.
3. Identifica al menos **dos recomendaciones** relacionadas con tu Storage Account (por ejemplo: *"Storage accounts should prevent shared key access"*, *"Storage account public access should be disallowed"*).
4. Para cada una, haz clic sobre la recomendación → revisa la pestaña **"Remediation steps"** → si el botón **"Fix"** está disponible, úsalo; si no, sigue los pasos manuales indicados.

> Recuerda: ya configuramos `allowBlobPublicAccess: false` y `minimumTlsVersion: 'TLS1_2'` directamente en la plantilla Bicep de la Parte 2 — esto es la diferencia entre Security by Default (nace seguro) y remediar después de que Defender lo detecta (lo que haces ahora manualmente). Compara cuántas recomendaciones NO aparecen justamente por esos dos parámetros que sí incluimos desde el diseño.

### 3.5 Registrar la evolución del Secure Score

Toma una captura de pantalla del Secure Score **antes** y **después** de remediar las recomendaciones del paso 3.4. La necesitarás para el informe final (Parte 6).

---

⬅️ Anterior: [Parte 2 · Landing Zone básica con Deployment Stacks](02-landing-zone-deployment-stacks.md) · 🏠 [README](README.md) · Siguiente ➡️: [Parte 4 · CSPM con Prowler](04-cspm-prowler.md)
