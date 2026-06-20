# ADR-0002 · Anti doble-agendamiento por reserva atómica del turno (primer commit gana)
**Estado:** aceptado
**Fecha:** 2026-06-20

## Contexto y fuerza
**R-06** exige impedir que dos pacientes queden en el mismo turno del mismo doctor,
y **R-17** exige que la disponibilidad nunca muestre turnos ya ocupados. **US-06**
ya fijó la **regla de negocio de desempate**: cuando dos reservas casi simultáneas
compiten por un turno, **gana la primera en confirmarse (primer commit / orden de
llegada)** y la otra se rechaza ofreciendo elegir otro turno. **US-01** refuerza
que un turno recién ocupado deja de aparecer como libre de inmediato. Esta es la
fuerza técnica central del delivery y el dolor `doble-agendamiento` de la
secretaria (`personas.md`).

## Decisión
Cada turno (doctor + fecha + hora) es un registro único en la base relacional. La
reserva se resuelve con una **escritura atómica que ocupa el turno**, garantizada
por una **restricción de unicidad** sobre (doctor, fecha, hora) para citas activas
(`pendiente`/`confirmado`) más el control de concurrencia transaccional de la base
(commit atómico). La **primera transacción que confirma la ocupación gana**; la
segunda viola la unicidad / encuentra el turno ocupado y se **rechaza** mostrando
que el turno ya no está libre y ofreciendo otro. La disponibilidad se calcula
leyendo turnos sin cita activa, por lo que un turno recién ocupado desaparece de
inmediato (R-17, US-01).

## Alternativas consideradas
- **Bloqueo optimista por versión sin restricción de unicidad** — válido, pero
  deja la integridad dependiente de la aplicación; la restricción de unicidad en la
  base es la red de seguridad más simple y barata contra el doble agendamiento.
- **Bloqueo a nivel de aplicación (lock en memoria / mutex)** — frágil ante más de
  un proceso o reinicio; no garantiza R-06 de forma duradera.
- **Reserva en dos fases con confirmación humana antes de ocupar** — abriría una
  ventana de doble reserva mientras recepción valida; en su lugar la cita ocupa el
  turno ya en estado `pendiente` (ver ADR-0004, retención de 24 h en US-02).

## Consecuencias
- **Ganamos:** garantía duradera de R-06 con una sola escritura atómica; la regla
  "primer commit gana" de US-06 queda implementada por el propio motor; R-17
  satisfecho sin lógica extra.
- **Costo aceptado:** la segunda reserva concurrente recibe un rechazo que la UI
  debe manejar con buen mensaje (ya contemplado en los criterios de US-06). La
  granularidad del turno (slots fijos por doctor) se asume como modelo de agenda
  del MVP; rangos personalizados y bloqueos del doctor (R-08) quedan fuera de
  alcance (open question / US-09 postergada).
