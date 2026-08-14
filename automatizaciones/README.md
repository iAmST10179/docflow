# Automatizaciones de Stuart OS

Las tres skills de briefing dejaron de ser skills y pasaron a ser **Routines** que corren en la nube.
Junto con una cuarta nueva que captura las reuniones de Granola.

Corren en servidor: da igual si la computadora está apagada.

## Las cuatro

| Routine | Cuándo (CST) | Cron (UTC) | Escribe en Notion |
|---|---|---|---|
| [Briefing Matutino](01-briefing-matutino.md) | 6:33 AM diario | `33 12 * * *` | Sí — mueve atrasadas |
| [Briefing Nocturno](02-briefing-nocturno.md) | 9:03 PM diario | `3 3 * * *` | No — sólo lee |
| [Revisión Semanal](03-revision-semanal.md) | Domingo 6:13 PM | `13 0 * * 1` | Sólo lo reversible |
| [Captura Granola](04-captura-granola.md) | 1:37 PM y 8:37 PM | `37 2,19 * * *` | Sí — eventos y tareas |
| [Asistente Ejecutivo](05-asistente-ejecutivo.md) | 8:47 AM, 2:47 PM, 9:47 PM | `47 3,14,20 * * *` | Sí — ejecuta tus respuestas |

Costa Rica es GMT-6 todo el año. El cron va en UTC: se le suman 6 horas a la hora local.
La semanal cae domingo 6:13 PM local, que en UTC ya es lunes 00:13 — por eso el día de semana del
cron es `1` y no `0`.

Granola corre **antes** que el nocturno, así el preview de mañana ya ve las tareas que salieron de
las reuniones del día.

## ⚠️ Paso manual pendiente: conectores

Las Routines quedaron creadas **sin conectores**. La API responde
*"the connectors parameter is not available for this organization"*, así que hay que
adjuntarlos a mano una sola vez:

1. Entrar a claude.ai → Routines
2. Por cada una de las cuatro, habilitar los conectores que usa:

| Routine | Conectores | Estado |
|---|---|---|
| Briefing Matutino | Notion · Google Calendar · Gmail | ✅ conectada |
| Briefing Nocturno | Notion · Google Calendar | ✅ conectada |
| Revisión Semanal | Notion · Google Calendar · Gmail | ✅ conectada |
| Captura Granola | Notion · Granola | ✅ conectada |
| **Asistente Ejecutivo** | **Notion** | ⚠️ **pendiente** |

Sin esto la Routine dispara, no encuentra las herramientas y no hace nada — y no falla ruidoso.

Actualizar el prompt de una Routine por API **no borra sus conectores**: se conservan. Sólo hay que
adjuntarlos una vez, cuando la Routine nace.

## La capa interactiva

Dos piezas nuevas en Notion, colgadas del **Centro de Operaciones** como sección 0:

- **Bandeja de decisiones** (`8b31a8cb-…`) — el único lugar donde Stu decide. Una fila por pregunta,
  con el contexto necesario para resolverla sin ir a buscar. Responde el select, el asistente ejecuta.
- **Estado del sistema** (`3bc82178-…`) — la memoria. A quién le espera qué, frentes vivos, decisiones
  ya tomadas, patrones y ruido conocido. Se lee al arrancar y se reescribe al terminar.

El matutino y la semanal **cuentan** lo pendiente pero no lo listan. Una sola superficie de decisión:
repetir las preguntas en tres lugares es pedirle a Stu que decida tres veces.

## Cambios de esquema en Notion

Dos propiedades nuevas, aditivas y reversibles:

- **Agenda → `Granola ID`** (texto). Clave de idempotencia de la captura de Granola.
  No se pudo reusar `ID externo`: lo ocupa el sync de Google Calendar y escribir ahí lo rompe.
- **Tareas → `Arrastres`** (número). Cuenta cuántas veces el matutino tuvo que rescatar la tarea.
  A las 3 deja de moverla y la congela.

## Qué se retira

`briefing-matutino`, `briefing-nocturno` y `revision-semanal` quedan obsoletas como skills: su lógica
vive ahora en la Routine. Mantenerlas en paralelo es la duplicación que `criterio-de-skills` prohíbe
— dos copias no se refuerzan, se desincronizan, y la vieja sobrevive en silencio.

Se pueden desactivar desde claude.ai una vez que las Routines corran una vuelta completa.

**No se tocan** `analizador-reuniones` ni `reunion-operativa`: siguen siendo las herramientas
manuales para contenido pegado, y la Routine de Granola le pasa la posta a `reunion-operativa`
en vez de repetir su criterio.

## Documentación desactualizada que quedó pendiente

Las referencias de `analizador-reuniones`, `reunion-operativa` y `memoria-de-uso` fueron verificadas
el 30 jun y ya no describen el workspace real. Lo verificado el 13 ago está en
[00-contexto-notion.md](00-contexto-notion.md). Las diferencias que importan:

- Agenda `Tipo` tiene **8** opciones, no 6: faltaban `Recordatorio` y `Huésped`.
- La plantilla `Seguimiento` (`2da82178…`) que citan **ya no existe**. Sólo queda `Nuevo Evento`.
- Agenda ganó `Calendario`, `ID externo`, `Alertas` y la relación `Notas`. Sí hay sync con Google
  Calendar, contra lo que dicen las skills.
- Tareas ganó `Origen` (Whagons / Compras).
- Proyectos tiene una relación `Meetings` → Agenda que ninguna skill usa.
- El **Habit Tracker** (`c25ae8db-…`) no estaba referenciado en ninguna skill.
