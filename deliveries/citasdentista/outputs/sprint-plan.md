# Sprint 1 — Que el paciente reserve solo sobre turnos reales y quede con la certeza de que su cita está agendada

**Capacidad:** 20 pts · **Comprometido:** 18 pts

| Historia | Pts | Épica | Prioridad |
|----------|-----|-------|-----------|
| US-01    | 5   | E-01  | 1         |
| US-02    | 5   | E-01  | 2         |
| US-03    | 3   | E-02  | 3         |
| US-05    | 5   | E-02  | 5         |

---

## Sprint Goal

> Que el paciente reserve solo sobre turnos reales y quede con la certeza de que su cita está agendada (y pueda liberarla a tiempo).

Este objetivo ataca el valor raíz del MVP: la **reserva autónoma sobre disponibilidad real** (E-01, prioridad 1) más el primer eslabón de la **certeza/liberación de la cita** (E-02). Sin esto no hay producto, y es el supuesto más arriesgado del descubrimiento ("los pacientes sí reservan solos en vez de seguir por WhatsApp").

## Por qué estas historias (criterio del Scrum Master)

Selección en orden estricto de prioridad, respetando la Definition of Ready, las dependencias y la capacidad:

- **US-01** (prio 1, sin dependencias) — base de todo: ver disponibilidad real.
- **US-02** (prio 2, depende de US-01 ✓ en sprint) — reservar con datos propios.
- **US-03** (prio 3, depende de US-02 ✓ en sprint) — comprobante de confirmación; cierra la certeza de la reserva.
- **US-05** (prio 5, depende de US-02 ✓ en sprint) — cancelar/reagendar por enlace; permite liberar el turno a tiempo.

## Historias evaluadas y NO incluidas (foco protegido)

- **US-04** (prio 4) — **excluida por dependencia no satisfecha en este sprint**: depende de US-03 **y US-05**; aunque ambas entran, sumarla rompería la capacidad (18 + 5 = 23 > 20). Queda como primera candidata del Sprint 2.
- **US-06** (prio 6, 5 pts) — no cabe en capacidad (18 + 5 = 23 > 20). No se sobre-compromete el sprint.
- **US-07** (prio 7) — no cabe en capacidad y habilita E-03/E-04; va a un sprint posterior.
- **US-08** (prio 8) — depende de US-07 (no en sprint) y no cabe en capacidad.

**Comprometido 18 pts ≤ capacidad 20 pts.** Todas las historias incluidas cumplen DoR/INVEST (`open_questions: []`, criterios de aceptación, estimación ≤ 8) y tienen sus dependencias resueltas dentro del propio sprint.
