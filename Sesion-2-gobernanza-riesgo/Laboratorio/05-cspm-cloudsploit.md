[🏠 README — Laboratorio 2](README.md) · Sección 5 de 8

## Parte 5 — CSPM con CloudSploit

### 5.1 Instalar CloudSploit

```powershell
mkdir C:\apps
cd C:\apps
git clone https://github.com/aquasecurity/cloudsploit.git
cd C:\apps\cloudsploit
npm install
```

> La instalación de dependencias (`npm install`) puede tardar varios minutos — CloudSploit tiene muchas dependencias para cubrir todos los proveedores cloud que soporta.

Verifica que la herramienta responde:

```powershell
node index.js -h
```

> 📸 **Captura para el informe — 5.1:** la salida de `node index.js -h` mostrando el menú de ayuda de CloudSploit.

### 5.2 Reutilizar o crear el Service Principal

Puedes reutilizar el mismo Service Principal `sp-lab2-jvargas-prowler` de la Parte 4 (ya tiene rol Reader), **o** crear uno independiente `sp-lab2-jvargas-cloudsploit` repitiendo los pasos 4.2 y 4.3 — la ventaja de usar uno separado es poder revocar el acceso de una herramienta sin afectar la otra. Para este laboratorio, **reutilizar el mismo Service Principal es suficiente**.

### 5.3 Configurar `config.js` y `azure.json`

1. Copia el archivo de configuración de ejemplo:

```powershell
cd C:\apps\cloudsploit
copy config_example.js config.js
```

2. Abre `config.js` con un editor de texto (VS Code, Bloc de notas) y busca la sección `azure:`. Modifícala para que quede así:

```javascript
    azure: {
        // OPTION 1: If using a credential JSON file, enter the path below
        credential_file: './azure.json',
        // OPTION 2 queda comentada, no la usamos
    },
```

3. En la misma carpeta `C:\apps\cloudsploit`, crea un nuevo archivo llamado exactamente `azure.json` con el siguiente contenido:

```json
{
  "ApplicationID": "<Application (client) ID>",
  "KeyValue": "<Value del secreto>",
  "DirectoryID": "<Directory (tenant) ID>",
  "SubscriptionID": "<tu ID de suscripción>"
}
```

> 🔒 Igual que con Prowler: **nunca subas `azure.json` a un repositorio**. Si este proyecto lo vas a subir a GitHub como evidencia, agrega `azure.json` a tu archivo `.gitignore` antes de tu primer `git add`.

> 📸 **Captura para el informe — 5.3:** el Explorador de Windows mostrando que `config.js` y `azure.json` existen en `C:\apps\cloudsploit` (basta con el listado de archivos — **no captures el contenido de `azure.json`**, contiene tu secreto).

### 5.4 Ejecutar el escaneo

```powershell
node index.js --config="C:\apps\cloudsploit\config.js" --cloud=azure --json=resultados.json --csv=resultados.csv --console=table --ignore-ok
```

**Explicación de las banderas:**

| Bandera | Qué hace |
| --- | --- |
| `--cloud=azure` | Le indica a CloudSploit que audite Azure (no AWS/GCP) |
| `--console=table` | Muestra los resultados en la terminal como tabla legible |
| `--ignore-ok` | Oculta los checks que **sí** pasaron, para enfocarte en los hallazgos |
| `--json=` / `--csv=` | Exporta los resultados completos a esos archivos, en la carpeta actual |

El escaneo toma normalmente **entre 2 y 5 minutos** para una suscripción del tamaño de este laboratorio.

> 📸 **Captura para el informe — 5.4:** la tabla de resultados en la terminal (`--console=table`) al finalizar el escaneo.

### 5.5 Revisar los resultados exportados

1. En `C:\apps\cloudsploit`, localiza el archivo `resultados.csv`.
2. Ábrelo con Excel (o Google Sheets).
3. Filtra la columna de estado (`status`) por `FAIL` para ver solo los hallazgos.
4. Compara: ¿los mismos hallazgos que reportó Prowler en la Parte 4 también aparecen aquí? ¿Hay hallazgos que **solo** detectó una de las dos herramientas? Anótalo — es exactamente el propósito de usar dos motores de reglas distintos (contrastar cobertura, como se explicó en la Sesión 2).

> 📸 **Captura para el informe — 5.5:** la tabla en Excel/Google Sheets filtrada por `status = FAIL`.

---

## Preguntas de repaso

1. ¿Qué ventaja pedagógica y de gobernanza tiene ejecutar **dos** herramientas CSPM distintas (Prowler y CloudSploit) sobre el mismo entorno?
2. ¿Reutilizaste el Service Principal de Prowler o creaste uno nuevo para CloudSploit? Justifica tu elección en términos de gobernanza de identidades.
3. Según tu comparación en el paso 5.5, ¿qué hallazgos detectaron **ambas** herramientas y cuáles fueron detectados por **solo una**? ¿Qué implica esto sobre confiar en una única herramienta CSPM?
4. ¿Por qué el archivo `azure.json` nunca debe subirse a un repositorio Git, incluso si el repositorio es privado?
5. Compara el formato de salida de CloudSploit (CSV/tabla) con el de Prowler (HTML/dashboard). ¿Cuál te resultó más útil para priorizar hallazgos y por qué?

---

⬅️ Anterior: [Parte 4 · CSPM con Prowler](04-cspm-prowler.md) · 🏠 [README](README.md) · Siguiente ➡️: [Parte 6 · Consolidación: informe de gobernanza y riesgo](06-consolidacion-informe.md)
