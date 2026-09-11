[← Volver al README](README.md) · Sección 7 de 7 · Parte C — Respuesta

# 7. RCA, informe ejecutivo del incidente y limpieza final de recursos

## Objetivo de la sección

Cerrar el ciclo de respuesta a incidentes con el análisis de causa raíz (RCA) y compilar el **informe ejecutivo — la entrega evaluada de esta sesión**. Después, ejecutar la limpieza obligatoria de recursos para no seguir consumiendo el crédito de Azure.

---

## 7.1 Análisis de causa raíz (RCA) — método de los 5 porqués

Completa esta cadena con tus propias palabras, basándote en el escenario de la Sección 4:

| # | Pregunta | Tu respuesta |
|---|---|---|
| 1 | ¿Por qué el atacante pudo acceder a la suscripción? | *Porque obtuvo credenciales válidas de un service principal.* |
| 2 | ¿Por qué esas credenciales estaban disponibles para el atacante? | *(completa)* |
| 3 | ¿Por qué el `appId`/`password` quedaron en un lugar accesible? | *(completa)* |
| 4 | ¿Por qué el proceso de despliegue usaba un secreto en texto plano en lugar de una identidad administrada? | *(completa)* |
| 5 | ¿Por qué no existía un control que detectara automáticamente la creación de un role assignment `Owner` fuera de un cambio planeado? | *(completa — causa raíz)* |

> 💡 Recuerda la diapositiva de RCA del Bloque 3: la causa inmediata ("credencial filtrada") casi nunca es la causa raíz. La causa raíz suele estar en un proceso o una ausencia de control, no en el evento puntual.

🧩 **Pregunta de repaso**: incluida como pregunta 7.1 más abajo — no la respondas aquí, resérvala para la plantilla del informe.

---

## 7.2 Plantilla del informe ejecutivo

Este es el documento que compilarás como tu entrega final. Cópialo en un procesador de texto y complétalo con tus propios resultados y capturas — **no lo entregues en blanco ni como una simple copia de esta guía.**

```markdown
# Informe Ejecutivo de Incidente — Sesión 5
**Estudiante:** _______________
**Fecha:** _______________
**Suscripción / Resource Group:** _______________

## 1. Resumen ejecutivo
(3-5 líneas: qué pasó, cuál fue el impacto, cómo se resolvió — escrito para un lector que
NO vio los logs)

## 2. Línea de tiempo del incidente
(Tabla con hora UTC, evento, fuente — basada en la consulta 5.4)

## 3. Análisis de logs
(Las 3 consultas KQL de la Sección 5, con sus capturas y una explicación de qué muestra
cada una)

## 4. Identificación del ataque — mapeo MITRE ATT&CK
(La tabla completada en el paso 5.5)

## 5. Contención y erradicación
(Los 4 pasos del playbook ejecutados en la Sección 6, con sus capturas de verificación)

## 6. Análisis de causa raíz (RCA)
(La cadena de 5 porqués del paso 7.1, completa)

## 7. Lecciones aprendidas y recomendaciones
(Mínimo 3 recomendaciones concretas — controles preventivos o de detección — que
reducirían el riesgo de que este incidente se repita)

## 8. Anexos
(Todas las capturas numeradas de las Secciones 1 a 6, en orden, con su etiqueta
"Captura para el informe — N.N")

## 9. Respuestas a las preguntas de repaso
(Las 28 preguntas de las Secciones 1 a 7, con sus respuestas)
```

---

## 7.3 Redactar las secciones 6 y 7 del informe

Para la sección "Lecciones aprendidas", basa tus recomendaciones en lo visto en **todo el curso**, no solo en esta sesión. Ejemplos de nivel de detalle esperado (no los copies literalmente — personalízalos con lo que observaste en tu propio laboratorio):

- Reemplazar el secreto en texto plano del service principal por una **Managed Identity** (visto en la Sesión 1 y 3), eliminando por completo la necesidad de manejar credenciales.
- Configurar una **Azure Policy** o una regla de alerta en Log Analytics que notifique automáticamente cada vez que se crea una asignación de rol `Owner` en la suscripción.
- Aplicar el principio de **Least Privilege**: el service principal original nunca debería haber tenido `Contributor` sobre todo el resource group si solo necesitaba desplegar contenedores.

---

## 🧩 Preguntas de repaso — Sección 7

1. ¿Por qué un informe ejecutivo de incidente debe poder leerse de principio a fin sin que el lector necesite ver los logs originales?
2. En tu cadena de 5 porqués (7.1), ¿en qué punto exacto la causa dejó de ser "un evento técnico puntual" y pasó a ser "una decisión de diseño o de proceso"?
3. De las tres recomendaciones de ejemplo en 7.3, ¿cuál habría evitado el incidente por completo (no solo detectarlo más rápido)? Justifica tu respuesta.
4. Si tuvieras que presentar este informe a un directivo sin formación técnica, ¿qué sección del documento cambiarías de redacción, y cómo?

---

## 7.4 Limpieza final de recursos (obligatoria)

> ⚠️ **No cierres este laboratorio sin completar esta sección.** Application Gateway y ACI siguen cobrando por hora hasta que se eliminan explícitamente — no basta con dejar de usarlos.

Ejecuta, en este orden:

```bash
# 1. Eliminar el Application Gateway (elimina también su configuración de WAF)
az network application-gateway delete \
  --resource-group <RG> \
  --name agw-dvwa-s5

# 2. Eliminar la IP pública asociada
az network public-ip delete \
  --resource-group <RG> \
  --name pip-agw-s5

# 3. Eliminar el contenedor DVWA
az container delete \
  --resource-group <RG> \
  --name dvwa-lab \
  --yes

# 4. Volver a deallocar las 4 VM de la Sesión 4 (como quedaron al cierre de esa sesión)
az vm deallocate --resource-group <RG> --name <nombre-vm-1> --no-wait
az vm deallocate --resource-group <RG> --name <nombre-vm-2> --no-wait
az vm deallocate --resource-group <RG> --name <nombre-vm-3> --no-wait
az vm deallocate --resource-group <RG> --name <nombre-vm-4> --no-wait
```

Verifica que todo quedó limpio:

```bash
az resource list --resource-group <RG> --output table
az vm list --resource-group <RG> --show-details --query "[].{Nombre:name, Estado:powerState}" --output table
```

Deberías ver: sin Application Gateway, sin IP pública `pip-agw-s5`, sin el contenedor `dvwa-lab`, y las 4 VM nuevamente en `VM deallocated`.

> 📝 **Sobre el Log Analytics Workspace**: puedes dejarlo — el volumen de datos de este laboratorio está muy por debajo del nivel gratuito (5 GB/mes), y lo necesitarás si el instructor pide evidencia adicional. Si de todas formas quieres eliminarlo: `az monitor log-analytics workspace delete --resource-group <RG> --workspace-name law-seguridad-nube-s5 --yes`.
>
> 📝 **Sobre el trial de Entra ID P1**: no requiere ninguna acción — expira automáticamente a los 30 días.

📸 **Captura para el informe — 7.1**: salida final de `az resource list` confirmando la limpieza, y de `az vm list` confirmando las 4 VM en `VM deallocated`.

🧪 **Checkpoint final 7.A**: confirma que no queda ningún recurso de tipo `Microsoft.Network/applicationGateways` ni `Microsoft.ContainerInstance/containerGroups` en tu resource group antes de cerrar la sesión de trabajo.

---

## Cierre del laboratorio

Con esto termina el laboratorio de la Sesión 5 — y con él, el recorrido completo de la especialización: de la responsabilidad compartida (Sesión 1) a una operación de seguridad cloud de extremo a extremo, con visibilidad, detección y respuesta reales.

**Recuerda:** compila el PDF único con todas las secciones, capturas y respuestas, y súbelo a Google Classroom antes de la fecha límite. Revisa la [`RUBRICA.md`](RUBRICA.md) antes de entregar para verificar que no te falta ningún componente evaluado.

---

[← Anterior: 6. Contención y erradicación](06-Contencion-y-Erradicacion.md) · [← Volver al README](README.md) · [Ver rúbrica de evaluación →](RUBRICA.md)
