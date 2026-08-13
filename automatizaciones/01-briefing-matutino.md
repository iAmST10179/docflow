# Automatización 1 — Briefing Matutino

**Corre:** todos los días 6:33 AM CST (`33 12 * * *` UTC) · **Entrega:** push + email

Limpia el arrastre y entrega el panorama del día. Nadie va a contestar del otro lado: no hay
preguntas, no hay confirmaciones, no hay turno de captura al final.

**Filosofía:** aliviar carga cognitiva, no amplificarla. Cruda, mínima, 15 segundos por sección.

---

## FASE 1 — Limpieza (silencio, sin confirmar)

Traer de **Tareas** todo lo que tenga `Fecha < hoy`, `Fecha` no nula y `Estado ≠ Completado`.
Por cada tarea, según su contador `Arrastres`:

| `Arrastres` | Qué hacer |
|---|---|
| 0, 1 o vacío | Mover `Fecha` a hoy · `Arrastres = Arrastres + 1` |
| 2 | Mover `Fecha` a hoy · `Arrastres = 3` · marcarla para la lista de congeladas de mañana |
| ≥ 3 | **No mover.** Borrar la `Fecha` (dejarla null) y listarla en 🧊 CONGELADAS |

Una tarea que se arrastró tres veces no se limpia moviéndola una cuarta. Se congela y se decide.

**Autorreparación:** si una tarea tiene `Arrastres > 0` y su `Fecha` es hoy o futura, Stu la
reactivó por su cuenta → poner `Arrastres = 0`. El contador mide rescates de la automatización,
no su trabajo.

Las tareas en `Estado = En espera` se mueven igual, pero no suben el contador: no dependen de él.

## FASE 2 — Datos del día (silencio, en paralelo)

1. **Agenda** — eventos de HOY con hora
2. **Google Calendar** — eventos de HOY (obligatorio, nunca omitir)
3. **Tareas** — con fecha de hoy y `Estado ≠ Completado`, ordenadas por prioridad
4. **Gmail** — no leídos de las últimas 14 horas (obligatorio, nunca omitir)

**Deduplicación:** Agenda es la fuente primaria, Google Calendar el espejo. Un evento que está en
ambos se muestra una vez. Lo que sólo esté en GCal va marcado `[solo GCal]`.

## FASE 3 — Salida (máx 300 palabras)

```
📅 HOY — [día, fecha]

⏰ AGENDA
[hora] [evento] → [preparación concreta, sólo si aporta]

🔥 URGENTE / ALTA
[tarea]

📋 HOY
[resto]

🧊 CONGELADAS — se arrastraron 3+ veces, decidí
[tarea] — [días desde que se creó] · ¿va o se archiva?

📧 CORREO
[asunto + remitente, sólo lo que exige acción tuya]

💡 PRIMERA ACCIÓN
[una sola — la que desbloquea más]

✅ [N] movidas a hoy · 🧊 [N] congeladas
```

Secciones vacías se omiten enteras, sin el encabezado. Si no hay congeladas, esa sección no aparece.
Si el correo no exige nada: una línea, `Nada que requiera acción`.

---

## Reglas

- **Sin preguntas de cierre.** Nada de "¿algo más que tengas en la cabeza?" — no hay nadie escuchando.
- **La limpieza no pide permiso.** Permiso permanente otorgado por Stu.
- **Lunes después de la revisión semanal:** no repetir el análisis de la semana. Confirmar en una
  línea que el plan sigue en pie y pasar al día.
- Gmail y Google Calendar son **sólo lectura**.
- Prohibido: "es importante tener en cuenta", "en resumen", voz pasiva, urgencia inventada.
