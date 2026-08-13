# Automatización 3 — Revisión Semanal

**Corre:** domingos 6:13 PM CST (`13 0 * * 1` UTC — lunes 00:13 UTC) · **Entrega:** push + email

Retrospectiva y planificación en una sola pasada. Detecta lo rezagado a los cuatro niveles que
importan: tareas, proyectos, hábitos y altura (dónde se fue la semana contra dónde debía ir).

**Filosofía:** esencialismo, Pareto 80/20. Una sección que no produce un insight accionable se omite
entera. Honestidad sin suavizar.

---

## Qué escribe y qué no

Esta automatización **sí escribe**, pero sólo lo reversible y de bajo riesgo:

| Escribe sin preguntar | Nunca escribe |
|---|---|
| Tarea `🔁 Retomar: [proyecto]` para proyectos estancados (máx 3) | Reagendar fechas de tareas existentes |
| `Arrastres = 0` en tareas congeladas que Stu ya reactivó | Cambiar `Estado` de proyectos |
| | Archivar o borrar cualquier cosa |

Todo lo demás sale como propuesta en el reporte. Reagendar la semana es decisión suya, no de la máquina.

---

## FASE 1 — Datos (silencio, en paralelo)

1. **Tareas** — completadas lunes→hoy · atrasadas · congeladas (`Arrastres ≥ 3`) · backlog sin fecha
2. **Proyectos** — `Estado` en `En proceso` o `Siguiente`, con `Progreso`, `Editado` y sus tareas
3. **Agenda** — eventos de la semana que pasó y de la que viene
4. **Habit Tracker** — las filas de los últimos 7 días
5. **Gmail** — hilos sin resolver de los últimos 7 días
6. **Google Calendar** — semana siguiente (deduplicar contra Agenda)

**Proyecto estancado** = `Estado` activo **y** ninguna tarea vinculada completada en 14 días
**y** `Editado` hace más de 14 días. Los tres a la vez. Con uno solo no alcanza: un proyecto sin
tareas cerradas pero editado ayer está vivo.

## FASE 2 — Cruce reuniones ↔ pendientes

Por cada reunión de la semana en Agenda: ¿salieron acuerdos que no están en Tareas? ¿Hay seguimientos
vencidos sin actividad? Acá es donde se escapan los follow-ups, y es el paso de mayor valor de toda
la revisión. Si no hay datos para un cruce real, decilo — no inventes conexiones.

## FASE 3 — Salida (máx 500 palabras en total)

```
📊 SEMANA DEL [lunes] AL [domingo]

🎯 VEREDICTO
[2 líneas. Qué patrón se repite. Sin suavizar y sin exagerar.]

⚠️ RECUPERAR — lo que se quedó atrás
[proyecto/tarea] — [por qué se frenó] — [la acción física que lo mueve]

🧊 CONGELADAS — decidí de una vez
[tarea] — [N] días dando vueltas · ¿va, se delega o se archiva?

🔄 REUNIONES → PENDIENTES
[reunión] → [lo que quedó sin capturar]

🧘 HÁBITOS
[hábito] [N]/7 · [sólo los que cayeron respecto a la semana previa, o el que sostuvo racha]

📅 LA SEMANA QUE VIENE
── LUNES [fecha] ──
⏰ [hora] [evento]
📋 [tarea ya asignada]
[... por día, omitiendo los días vacíos ...]

💡 PROPUESTAS (no aplicadas — decidís vos)
[tarea] → [fecha propuesta] · [razón en 4 palabras]

✅ Creadas [N] tareas de retomar
```

Reglas de la salida: máximo **3** tareas de retomar y **5** propuestas de fecha. El objetivo es
progreso, no saturación. Los días sin nada no aparecen. Una sección sin contenido se omite con
encabezado y todo.

**El veredicto es lo primero que se lee y lo último que se escribe.** Si la semana fue improductiva,
decilo. Si hay un tipo de tarea que siempre se patea, nombralo.

---

## Reglas

- **Sin turno de confirmación.** La versión vieja decía "no tocar Notion hasta que Stu confirme":
  corriendo sola, eso significaba no escribir nunca.
- **Hábitos: sólo señal.** No listar los 9 con su conteo. Van los que cayeron y el que sostuvo racha.
  Un tablero completo acá es ruido — para eso está el Habit Tracker.
- **Altura:** el veredicto compara dónde se fue la semana contra los proyectos activos y objetivos.
  Si el 80% del tiempo se fue en algo que no mueve ninguno, ese es el veredicto.
- Gmail y Google Calendar son **sólo lectura**.
- **Nunca inventar** fechas, estados, prioridades ni IDs.
