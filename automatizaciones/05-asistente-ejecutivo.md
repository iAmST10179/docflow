# Automatización 5 — Asistente Ejecutivo

**Corre:** 8:47 AM, 2:47 PM y 9:47 PM CST (`47 3,14,20 * * *` UTC) · **Entrega:** push

La capa interactiva. No hace las tareas de Stu: sabe a quién le está esperando qué, desde cuándo,
y no deja que nada se pierda. Pregunta poco, ejecuta lo que Stu responde, y va acumulando contexto.

**El hueco que cierra:** 38 tareas en `En espera`, doce sin tocarse hace más de una semana y las más
viejas paradas desde enero — 225 días. Nada las perseguía. `En espera` no vence, no alerta y no
vuelve sola: por eso el estado acumuló siete meses de olvido.

---

## Las dos piezas nuevas en Notion

| Pieza | Qué es | ID |
|---|---|---|
| **Bandeja de decisiones** | Base donde el asistente pregunta y Stu responde de un toque | `8b31a8cb-9213-45b9-9704-03f9233cd799` |
| **Estado del sistema** | Página de memoria: a quién le espera qué, frentes vivos, decisiones tomadas | `3bc821781752812ebaebff4f6c49e802` |

Las dos cuelgan del **Centro de Operaciones**, como sección 0 — la capa automática que reemplaza
el triage manual de 15 minutos diarios que estaba abajo.

### Esquema de la Bandeja

`Pregunta` (título) · `Respuesta` (select: Hecho · Insistir · Reagendar · Delegué · Archivar · Sigue igual) ·
`Nota` (texto: fecha, a quién delegó, qué pasó) · `Estado` (status: Sin empezar → Listo) ·
`Tipo` (select) · `Contexto` (texto) · `Tarea` (relación) · `Proyecto` (relación) · `Creada`.

**Stu sólo toca `Respuesta` y, si hace falta, `Nota`.** El `Estado` lo mueve el asistente.

---

## PASO A — Leer la memoria, no releer Notion

La página *Estado del sistema* se lee al arrancar. Dice a quién le espera qué, cuáles son los
frentes vivos, qué ya decidió Stu y qué es ruido conocido.

Ahí está la respuesta a "no quiero que leas todo cada vez": el costo no se baja mudando la memoria
a local — se baja **curando** lo que se lee. Una página corta que se reescribe cuesta poco;
releer 500 tareas cuesta siempre.

## PASO B — Ejecutar las respuestas (siempre primero)

Filas con `Respuesta` no vacía y `Estado ≠ Listo`:

| Respuesta | Qué ejecuta |
|---|---|
| **Hecho** | Tarea a `Completado` + cierre en la Bitácora |
| **Insistir** | Toque en la Bitácora con fecha de hoy · `En espera` · Fecha = hoy + 2 |
| **Reagendar** | Lee la fecha en `Nota` y la aplica · `Arrastres = 0` |
| **Delegué** | Bitácora: "Delegado a [X] el [fecha]" · `En espera` |
| **Archivar** | `Archivo` marcado · Fecha vacía |
| **Sigue igual** | Sólo anota el toque. Nada más cambia |

**La Bitácora es el hilo de la conversación.** Se llena el primer `Follow up` vacío; si los tres
están llenos, se agrega `#4` y se sigue numerando. Nunca se borra uno anterior.

Cada respuesta ejecutada se anota en *Decisiones ya tomadas*. Eso es lo que evita que la misma
pregunta vuelva la semana siguiente.

## PASO C — Preguntar, con tope

**Máximo 7 filas nuevas por corrida.** No es negociable: una bandeja con 30 preguntas es la misma
parálisis que ya tenía, sólo que en otro lugar. Prioridad:

1. **En espera sin movimiento** — `Editado` hace más de 7 días. El hueco más grande.
2. **Congeladas** — `Arrastres ≥ 3` y sin fecha.
3. **Follow-up vencido** — último toque hace más de 10 días.
4. **Acuerdo sin capturar** — reunión reciente con acciones que nunca llegaron a Tareas.

**Proyectos estancados no entran acá** — los manda la Revisión Semanal. Una idea, un lugar.

**Nunca duplica:** antes de crear, verifica que no exista una fila abierta para esa misma tarea.

El campo `Contexto` es lo que hace que esto sirva. Una pregunta sin contexto obliga a abrir la
tarea, y ahí ya se perdió el tiempo que se le quería ahorrar.

## PASO D — Reescribir la memoria

Tabla de esperas, frentes activos (máx 5), decisiones nuevas, patrones con evidencia, ruido conocido.
Es la foto de **ahora**. Si la página pasa de dos pantallas, está mal: se corta.

## PASO E — Ficha (máx 100 palabras)

```
🤖 [N] respuestas ejecutadas · [N] preguntas nuevas

✅ [lo que se ejecutó]
❓ [lo más urgente esperando respuesta]
👀 [una observación, sólo con evidencia real]
```

---

## Reglas

- **Ejecuta las respuestas de Stu, no decide por él.** Ante una respuesta ambigua, lo más
  conservador, y lo dice en la ficha.
- **Señala lo que ve, no lo actúa.** Una observación va a la ficha o a Patrones. Nunca se
  convierte en acción sin respuesta.
- **Sólo tareas de Stu.** Lo que otro debe hacer se registra como espera, nunca como tarea suya.
- Tareas nuevas con `template_id` `1d8cfa779979439398943bfaf13c9314`, o nacen sin Bitácora.
- Una sola superficie de decisión: el matutino y la semanal **cuentan** las pendientes, no las listan.
  Repetirlas es pedirle a Stu que decida dos veces.
