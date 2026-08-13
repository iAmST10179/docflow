# Automatización 4 — Captura de Granola → Notion

**Corre:** 1:37 PM y 8:37 PM CST (`37 2,19 * * *` UTC) · **Entrega:** push + email

Barre Granola, encuentra las reuniones que todavía no tienen su análisis en Notion, y las escribe
completas: evento en Agenda con el cuerpo lleno y las tareas de Stu creadas y vinculadas.

**Ninguna reunión queda afuera.** Que haya sido ayer o hace dos semanas no la vuelve menos
importante: la información y las tareas siguen vivas.

---

## PASO 1 — Barrido e idempotencia

Traer **siempre** la ventana `last_30_days` completa, en las dos corridas del día. No es caro y es
lo que garantiza que una reunión de hace ocho días que nunca se procesó entre igual.

Por cada reunión, decidir en este orden:

1. **¿Existe un evento en Agenda con `Granola ID` = el id de la reunión?** → ya está procesada, saltar.
2. **¿Hay un evento en Agenda del mismo día cuya hora caiga a ±90 min?** → es el mismo, **actualizar
   ese** y escribirle el `Granola ID`. Si el evento no tiene hora, basta el mismo día más un
   parecido razonable de título o de participantes.
3. **Si no hay nada** → crear el evento nuevo.

**Nunca matchear por título exacto.** Los títulos no coinciden nunca y matchear así duplica:
Granola dice `Operativa 11/08/26`, Notion dice `Reunión Operativa`. Granola dice
`Reunión con Josué 05/08/26`, Notion dice `Reunión con Josué para revisar Diversos Temas`.
La fecha y la hora son la llave; el título es apenas desempate.

**El `Granola ID` se escribe siempre**, tanto al crear como al actualizar. Es lo único que evita
que la próxima corrida vuelva a procesar la misma reunión.

⚠️ **`ID externo` no se toca nunca** — es del sync de Google Calendar. Escribirlo ahí rompe el sync.

## PASO 2 — Operativas: no las proceses acá

Si el título trae `operativa` (en cualquier forma: `Operativa 11/08/26`, `Reunión Operativa 24/07/26`),
es la junta diaria del hotel → **pasale la posta a la skill `reunion-operativa`**, que tiene el criterio
específico (ignorar cifras del reporte, ignorar el tramo de TripAdvisor, sacar sólo observaciones de
mejora y acotaciones de cierre). Escribí el `Granola ID` igual cuando termine.

La detección por carpeta de Granola que esa skill usaba **ya no funciona**: la API responde
*"Meeting folders are only available to paid Granola tiers"*. Por eso se detecta por título.

## PASO 3 — No pisar lo que ya está escrito a mano

Antes de escribir el cuerpo, mirá qué tiene ya el evento:

- **Si ya trae un análisis** — cualquier sección de decisiones, acciones, acuerdos, seguimiento,
  esencia, saldo o próximos pasos → **sólo escribir el `Granola ID` y no tocar el cuerpo.**
  Reportarla como `ya tenía análisis propio`.
- **Si trae contenido suelto que no es análisis** (una lista de participantes, una nota corta) →
  insertar el análisis con `position: start`. Prepende y lo de abajo sobrevive intacto.
- **Si está vacío** → insertar normal.

Esto no es una optimización, es una salvaguarda. Stu escribe estos análisis a mano y le salen
mejores que la plantilla: su versión de la reunión con Pedro Corella abre con el saldo, cierra con
los cabos sueltos y trae datos que Granola ni registró. Reprocesarla la habría ensuciado.

**Nunca borrar. Nunca reemplazar. Sólo prepender, y sólo cuando no hay análisis.**

## PASO 4 — Perfil de la reunión

El perfil se deduce de la dinámica que se ve en la transcripción, no de una lista de nombres:

| Señal en la conversación | Perfil |
|---|---|
| Stu asigna trabajo, pide avances, corrige ejecución | **Mi equipo** |
| A Stu le piden cuentas, le bajan objetivos o decisiones | **Gerencia** |
| Intercambio entre pares, se coordinan sin jerarquía | **Compañeros** |
| Hay alguien externo con interés comercial (cliente, huésped, proveedor, agencia, entrevista) | **Cliente / externo** |
| Contexto no laboral | **Personal** |
| Alguien enseña y Stu aprende (capacitación, clase, inducción) | **Formación** |

Si la transcripción no alcanza para decidir, usá **Compañeros** y marcá el perfil con ❓ en la ficha.
Nunca inventes una jerarquía que no se oyó.

**Formación es el perfil tramposo:** una capacitación de dos horas puede no dejar ni una sola tarea,
y está bien. Su valor es conocimiento, y el conocimiento va a **Notas** vía `conector-conocimiento`,
no a Tareas. Forzar tareas para justificar la corrida es exactamente lo que ensucia el sistema.
Del cuerpo salen sólo los acuerdos operativos que sí le tocan a Stu.

## PASO 5 — Cuerpo del evento

Una sola plantilla de Agenda (`Nuevo Evento` = `68a60c82726b4c8f8309f75965bba340`). El núcleo es
igual para todos los perfiles y **sólo cambia un bloque** — por eso no hacen falta cinco plantillas
en Notion: el 90% se repetiría.

```markdown
# 🎯 [Verbo + resultado concreto]
**Impacto:** [qué cambió o qué se bloqueó. Una línea.]
**Decisión:** [lo que quedó en firme, o "Ninguna".]

## ☑️ Mis acciones
| # | Qué | Prioridad | Fecha |
|---|-----|-----------|-------|

## [BLOQUE VARIABLE — según perfil]

## 📌 Compromisos de otros
[quién · qué · para cuándo — registro, NO se convierten en tareas]

## 🧩 Cabos sueltos
[lo que quedó sin respuesta y bloquea algo. Omitir si no hay.]
```

*Cabos sueltos* sale del propio formato manual de Stu y es la sección que más valor rescata:
lo que nadie contestó es lo que después frena todo.

El bloque variable:

- **Cliente / externo** → `## 🤝 Compromiso asumido` — qué prometí, para cuándo, qué pone en riesgo la relación, cuándo es el próximo contacto.
- **Gerencia** → `## 📈 Rendición` — qué me pidieron y con qué fecha, qué debo reportar de vuelta, qué riesgo tengo que escalar.
- **Mi equipo** → `## 👥 Seguimiento de equipo` — qué instruí, qué debo verificar y cuándo, señales de carga o clima.
- **Compañeros** → `## 🔁 Qué necesito de ellos` — qué espero, de quién, para cuándo.
- **Personal** → `## 📝 Acuerdos` — qué quedó y qué me toca.

Se escribe con `insert_content` en `position: {type:"start"}` para no romper el bloque sincronizado
ni las tareas inline que la plantilla trae al final.

## PASO 6 — Tareas

**Sólo lo que Stu ejecuta.** Lo que otro tiene que hacer va al bloque *Compromisos de otros* del
cuerpo y no genera fila en Tareas.

Un seguimiento sí es tarea de Stu cuando **él** tiene que perseguirlo, y se nombra como acción suya:
`🔄 Preguntarle a [nombre] por [qué]` — nunca `[Nombre] debe entregar X`, que es la tarea de otro
disfrazada.

Campos: `Estado` = `Por hacer` · `Contexto` = `Trabajo` (o `Personal` según perfil) ·
`Evento` = el evento de Agenda de esta reunión · `Area` y `Proyecto` cuando estén claros, ❓ si no ·
`Prioridad` y `Fecha` de lo que se dijo, nunca inventadas.

Filtro de supervivencia: lo que se resolvió durante la llamada no genera tarea. Sólo lo que sigue vivo.

## PASO 7 — Ficha (máx 120 palabras)

```
📥 GRANOLA — [N] reuniones procesadas

[Reunión] · [perfil] · [creada|actualizada] → [link]
  ☑️ [N] tareas

⏭️ Sin cambios: [N] ya procesadas
❓ [lo que quedó sin resolver, o "Nada"]
```

Si no había nada nuevo, una línea: `Sin reuniones nuevas.` Nada más.

---

## Reglas

- **Ventana de 30 días es el techo real.** Granola sólo acepta `this_week`, `last_week` y
  `last_30_days`. Una reunión de hace 40 días ya no es recuperable por API — si hace falta, se pega
  a mano y la agarra `analizador-reuniones`.
- **Verificar con fetch después de escribir.** Confirmar que quedó en la base correcta, que la
  plantilla se aplicó y que el `Granola ID` está escrito. Un `Granola ID` que no se guardó significa
  que la próxima corrida duplica.
- **Prohibido en la redacción:** "se discutió", "se mencionó", "se revisará", voz pasiva.
  Verbos de acción: Validar · Enviar · Cortar · Aprobar · Confirmar · Escalar.
- Máximo 2 líneas por punto. Sin acciones reales → `Operación estable.`
