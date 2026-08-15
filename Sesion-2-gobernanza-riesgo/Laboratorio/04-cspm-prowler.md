[🏠 README — Laboratorio 2](README.md) · Sección 4 de 8

## Parte 4 — CSPM con Prowler

### 4.1 Instalar Prowler

```powershell
pip install prowler
```

> **¿Por qué `pip` y no `pipx`?** Ambas opciones son válidas. `pipx` es la práctica recomendada en general porque aísla cada herramienta de línea de comandos en su propio entorno virtual, evitando conflictos de dependencias con otros proyectos Python que tengas instalados. Para este laboratorio usamos `pip` directo porque es un paso menos que explicar y suficiente si tu equipo no tiene otros proyectos Python complejos. Si prefieres la práctica más robusta, usa en su lugar:
>
> ```powershell
> python -m pip install --user pipx
> python -m pipx ensurepath
> # cierra y vuelve a abrir la terminal antes de continuar
> pipx install prowler
> ```

Verifica la instalación:

```powershell
prowler -v
```

> **Si PowerShell no reconoce el comando `prowler`:** el ejecutable no está en el `PATH` del sistema. Ejecuta `pip show prowler`, copia el valor del campo `Location`, y agrega esa ruta (reemplazando `site-packages` por `Scripts`) a la variable de entorno `PATH` de Windows siguiendo [esta guía](https://www.neoguias.com/agregar-directorio-path-windows/).

### 4.2 Crear la identidad de solo lectura (Service Principal)

Prowler necesita autenticarse contra Azure. Vamos a crear una identidad de aplicación (Service Principal) exclusiva para auditoría, **sin permisos de escritura**.

1. En el portal, busca **"Microsoft Entra ID"**.
2. En el menú izquierdo, ve a **App registrations** → **+ New registration**.
3. Completa:
   - **Name:** `sp-lab2-jvargas-prowler`
   - **Supported account types:** deja la opción por defecto (*Accounts in this organizational directory only*).
4. Haz clic en **Register**.
5. En la página de la aplicación recién creada, copia y guarda en un archivo de texto temporal:
   - **Application (client) ID**
   - **Directory (tenant) ID**
6. En el menú izquierdo, ve a **Certificates & secrets** → pestaña **Client secrets** → **+ New client secret**.
7. En **Description** escribe `prowler-lab2`, deja la expiración por defecto (o reduce a 90 días, ya que es solo para el laboratorio) y haz clic en **Add**.
8. **Copia inmediatamente el valor (`Value`) del secreto** — Azure solo lo muestra una vez. Guárdalo en tu archivo temporal.

> 📸 **Captura para el informe — 4.2:** la página de la aplicación `sp-lab2-jvargas-prowler` mostrando el **Application (client) ID** y el **Directory (tenant) ID**. **No incluyas el valor del secreto en la captura** — solo su nombre/descripción.

> 🔒 **Seguridad:** nunca subas este archivo con las credenciales a un repositorio de Git ni lo compartas. Al finalizar el laboratorio, elimina el Service Principal (ver Parte 7).

### 4.3 Asignar el rol Reader sobre la suscripción

1. En el portal, busca **"Suscripciones"** y entra a tu suscripción Free Tier.
2. En el menú izquierdo, ve a **Access control (IAM)**.
3. Haz clic en **+ Add** → **Add role assignment**.
4. En la pestaña **Role**, busca y selecciona **Reader**. Haz clic en **Next**.
5. En **Assign access to**, deja **User, group, or service principal**.
6. Haz clic en **+ Select members**, busca `sp-lab2-jvargas-prowler` por su nombre, selecciónalo y haz clic en **Select**.
7. Haz clic en **Review + assign** (dos veces, para confirmar).

> 📸 **Captura para el informe — 4.3:** la lista de "Role assignments" en IAM mostrando `sp-lab2-jvargas-prowler` con el rol **Reader**.

**Equivalente en Azure CLI** (opcional):

```powershell
$APP_ID = "<pega-aquí-el-Application-client-ID>"

az role assignment create `
  --assignee $APP_ID `
  --role "Reader" `
  --scope "/subscriptions/$SUBSCRIPTION_ID"
```

> ✅ **Por qué `Reader` y nunca `Contributor` u `Owner`:** una auditoría de postura solo necesita **leer configuración**, nunca modificarla. Asignar el mínimo privilegio necesario (Least Privilege, Sesión 1) es una buena práctica que además reduce el riesgo si el secreto llegara a filtrarse.

### 4.4 Configurar las variables de entorno

En tu terminal de PowerShell (la misma sesión donde ejecutarás Prowler):

```powershell
$env:AZURE_CLIENT_ID = "<Application (client) ID>"
$env:AZURE_TENANT_ID = "<Directory (tenant) ID>"
$env:AZURE_CLIENT_SECRET = "<Value del secreto>"
```

> Estas variables solo existen mientras esa ventana de PowerShell permanezca abierta. Si la cierras, deberás volver a ejecutarlas.

### 4.5 Ejecutar el escaneo

```powershell
prowler azure --sp-env-auth
```

Prowler comenzará a ejecutar cientos de checks contra tu suscripción usando el framework por defecto (CIS). Verás una barra de progreso en la terminal. El escaneo de una suscripción pequeña como la de este laboratorio toma normalmente **entre 3 y 8 minutos**.

Si prefieres especificar un framework de cumplimiento explícito (recomendado, para conectarlo con la teoría de gobernanza de la Sesión 2):

```powershell
prowler azure --sp-env-auth --compliance cis_4.0_azure
```

Otras banderas útiles:

```powershell
# Ver todos los checks disponibles sin ejecutarlos
prowler azure --sp-env-auth --list-checks

# Generar explícitamente varios formatos de salida
prowler azure --sp-env-auth -M csv json html
```

> 📸 **Captura para el informe — 4.5:** la terminal mostrando el escaneo en progreso o el resumen final (tabla de hallazgos por severidad) al terminar.

### 4.6 Revisar el reporte HTML

1. Al finalizar, Prowler indica en la terminal la carpeta donde quedaron los resultados (por defecto: `./output/`).
2. Navega a esa carpeta desde el Explorador de Windows.
3. Abre el archivo `.html` con doble clic (se abre en tu navegador).
4. Explora el resumen: verás los hallazgos agrupados por severidad (`critical`, `high`, `medium`, `low`) y por servicio.
5. Identifica los **3 hallazgos de mayor severidad** relacionados con tu Storage Account o tu suscripción — los necesitarás para el informe final.

> 📸 **Captura para el informe — 4.6:** el resumen del reporte HTML (gráfico de hallazgos por severidad) y una captura adicional de los 3 hallazgos de mayor severidad identificados.

### 4.7 Explorar el Prowler Dashboard (opcional)

Prowler incluye un dashboard interactivo local:

```powershell
prowler dashboard
```

La terminal mostrará una URL local (típicamente `http://127.0.0.1:11666`). Ábrela en tu navegador para explorar los hallazgos de forma interactiva, con filtros por severidad, servicio y framework de cumplimiento.

> 📸 **Captura para el informe — 4.7 (opcional):** el Prowler Dashboard mostrando el resumen visual de hallazgos.

---

## Preguntas de repaso

1. ¿Por qué el Service Principal de Prowler solo tiene el rol **Reader** y nunca **Contributor** u **Owner**?
2. ¿Qué riesgo existe si el `AZURE_CLIENT_SECRET` se filtra, y qué medida de este laboratorio ayuda a mitigarlo?
3. ¿A qué framework de cumplimiento mapea Prowler los hallazgos cuando usas `--compliance cis_4.0_azure`? ¿Cómo se relaciona esto con el CCM de la CSA visto en la Sesión 2?
4. De los 3 hallazgos de mayor severidad que identificaste, ¿cuál tratamiento de riesgo (mitigar/transferir/aceptar/evitar) le asignarías a cada uno?
5. ¿Prowler detectó algún hallazgo relacionado con la etiqueta `ambiente` de la Parte 1, o con la configuración del Storage Account de la Parte 2? ¿Por qué sí o por qué no?

---

⬅️ Anterior: [Parte 3 · Microsoft Defender for Cloud (Secure Score)](03-defender-for-cloud.md) · 🏠 [README](README.md) · Siguiente ➡️: [Parte 5 · CSPM con CloudSploit](05-cspm-cloudsploit.md)
