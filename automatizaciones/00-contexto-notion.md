# Contexto Notion — verificado 13 ago 2026

Fuente de verdad de IDs y esquemas para las cuatro automatizaciones. Verificado contra el
workspace real, no copiado de las referencias viejas (que estaban desactualizadas desde el 30 jun).

## IDs de escritura (`data_source_id`) — el parent SIEMPRE es uno de estos

| Base | `data_source_id` |
|---|---|
| Agenda | `07db137e-7dc1-42bc-8735-a0f6c42ace8b` |
| Tareas | `362af354-510e-4917-a310-cd9ac47843f3` |
| Proyectos | `91fff4aa-9910-448e-a653-71ef4c4cd852` |
| Áreas | `bdbb85f1-e351-403e-93a3-096f4c9fafa1` |
| Objetivos | `bb4a5a5f-315d-4d1f-b02c-ad0487a5cb1b` |
| Notas | `8ad7388c-be63-487a-91dd-21733a2bd5f3` |
| **Habit Tracker** | `c25ae8db-5755-445a-a70e-9c37b92ad418` |

Crear con el ID de página-contenedor como parent = página huérfana con las propiedades perdidas.

**Páginas clave:** Área Tabacón `2c18217817528060bb4bd9cf16a71f38` · Dashboard `888b5dea415d4175917a15bef55ed0cf` · Memoria de Uso `391821781752811db500f40af67c8656`

**Plantilla de Agenda:** `Nuevo Evento` = `68a60c82726b4c8f8309f75965bba340` — es la única que existe.
La plantilla `Seguimiento` (`2da82178…`) que citaban `analizador-reuniones` y `reunion-operativa` fue borrada.

## Esquemas

**Agenda** — `Nombre` (título) · `Tipo` (select, 8) · `Fecha` (date, hora vía `is_datetime`, GMT-6) ·
`Enlace de la reunión` (url — acá vive la ubicación) · `Descripción` (texto) · `Alertas` (texto) ·
`Calendario` (select: Manual/Google/Familia/Outlook UCAT/iCloud) · `ID externo` (texto — **lo usa el sync
de Google, no tocar**) · `Granola ID` (texto — nuevo, clave de idempotencia) ·
relaciones `Area` `Proyecto` `Tareas` `Notas`.

`Tipo`: `Recordatorio · Reunión · En seguimiento · Personal · Clase / Estudio · Trabajo · Cumpleaños · Huésped`

**Tareas** — `Nombre` (título) · `Estado` (status: `Clasificación · Por hacer · En espera · Completado`) ·
`Prioridad` (select: `Baja · Media · Alta · Urgente`) · `Contexto` (multi: `Personal · Trabajo · Universidad`) ·
`Origen` (multi: `Whagons · Compras`) · `Fecha` (date) · `Arrastres` (número — nuevo, contador anti-zombie) ·
`Descripción` · `userDefined:URL` · `Archivo` (checkbox) · `Creado` / `Editado` (automáticos) ·
relaciones `Area` `Proyecto` `Objetivo` `Evento` `Notas` `Cursos`.

**Proyectos** — `Estado` (`Clasificación · Siguiente · En proceso · En espera · Completado`) ·
`Prioridad` · `Fecha de entrega` · `Departamento` (multi) · `Progreso` (rollup, no escribir) ·
`Editado` (last_edited_time — sirve para detectar estancamiento) ·
relaciones `Area` (límite 1) `Tareas` `Meetings`→Agenda `Objetivo` `Proyecto padre` `Sub-proyectos` `Notas` `Referencias` `Curso`.

**Habit Tracker** — `Día` (título) · `Fecha` (date) · `Progress` (fórmula, no escribir) ·
checkboxes: `6-10` · `Ayuno` · `Deporte` · `Ducha fría` · `Escribir` · `Leer` · `Meditar` · `Movilidad` · `Rezar/Agradecer` ·
relación `Area`.

**Áreas activas (9):** Tabacón · Psicología - UCAT · Psicología - Profundización personal ·
Métodos de estudio y aprendizaje · Crecimiento personal y espiritualidad · Ocio y creatividad ·
Salud y bienestar · Desarrollo profesional · Aprendizaje y cultura.

## Reglas de escritura que nunca se rompen

1. **Parent = `data_source_id`.** Nunca el ID de la página-contenedor.
2. **Plantilla al crear** (`template_id` en la creación), nunca aplicada después: si no, se pierden
   el bloque sincronizado y la base de tareas inline.
3. **Cuerpo vía `insert_content` con `position: {type:"start"}`.** Entra arriba y deja intactos el
   bloque sincronizado y las tareas inline que la plantilla trae al final.
4. **Relaciones = array JSON de URLs de página.**
5. **Nunca inventar** fechas, estados, áreas, prioridades ni IDs. Ante la duda, ❓.
6. **Nada en silencio.** Un rate limit o un error que deja algo a medias se reporta con nombre y apellido.

## Zona horaria

Costa Rica = GMT-6 todo el año, sin horario de verano. Los cron de las Routines van en **UTC**:
sumar 6 horas a la hora local.
