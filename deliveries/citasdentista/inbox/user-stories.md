# User Stories — citasdentista

> Generado desde: `personas.md`, `requisitos.md`, `evidence-map.json`.
> Toda historia es INVEST, cita su fuente y rastrea el/los requisito(s) que la originan.
> Orden: primero el **núcleo de valor** (reserva autónoma sobre una sola fuente de
> verdad + confirmación/recordatorio), que es lo que más duele y más se repite
> entre las tres personas.

---

## Núcleo de valor — lo imprescindible del MVP

### Paciente

- **[US-01]** Como **paciente**, quiero **ver los horarios realmente disponibles por doctor y día** para **elegir uno sin tener que preguntar uno a uno y esperar respuesta**.
  - Criterios de aceptación:
    - Dado que abro el enlace de reserva, cuando selecciono un doctor y un día, entonces veo solo los turnos libres de ese doctor para ese día.
    - Dado que un turno acaba de ser ocupado por otra persona, cuando consulto la disponibilidad, entonces ese turno ya no aparece como libre.
    - Dado que no hay turnos libres en el día elegido, cuando lo selecciono, entonces el sistema me lo indica y me ofrece el siguiente día con cupo.
  - Trazabilidad: R-01, R-17 · Dolores: `sin-visibilidad-horarios`, `desconfianza-disponibilidad`
  - Fuente: `paciente.md`, `secretaria.md`

- **[US-02]** Como **paciente**, quiero **reservar mi cita desde un enlace ingresando yo mismo mis datos** para **no depender de un chat largo con recepción y agendar en pocos pasos desde el celular**.
  - Criterios de aceptación:
    - Dado que elegí doctor, horario y motivo, cuando completo nombre, cédula, teléfono y correo, entonces el sistema crea la cita en estado `pendiente` y la asocia a ese turno.
    - Dado que envío la reserva, cuando recepción la revisa, entonces los datos ya están capturados y solo los valida (no los recoge de nuevo).
    - Dado que es una cita de control y no primera vez, cuando reservo, entonces el sistema no me exige más datos de los necesarios.
  - Trazabilidad: R-02, R-13, R-14 · Dolores: `sin-visibilidad-horarios`, `recopilacion-datos-repetitiva`
  - Fuente: `paciente.md`, `secretaria.md`

- **[US-03]** Como **paciente**, quiero **recibir un comprobante de confirmación apenas reservo** para **quedar seguro de que la cita quedó agendada y no con la duda**.
  - Criterios de aceptación:
    - Dado que completo la reserva, cuando el sistema la registra, entonces recibo de inmediato un comprobante con doctor, fecha, hora y motivo.
    - Dado que recibí el comprobante, cuando llego a la clínica, entonces mi cita aparece en el sistema con los mismos datos del comprobante.
  - Trazabilidad: R-03 · Dolores: `incertidumbre-confirmacion`, `desconfianza-disponibilidad`
  - Fuente: `paciente.md`

- **[US-04]** Como **paciente**, quiero **recibir un recordatorio automático antes de la cita** para **no olvidarla y poder cancelar a tiempo si no podré asistir**.
  - Criterios de aceptación:
    - Dado que tengo una cita confirmada, cuando se acerca la fecha, entonces recibo un recordatorio con fecha, hora, doctor e instrucciones previas (llegar antes, llevar exámenes, etc.).
    - Dado que recibo el recordatorio, cuando no podré asistir, entonces el mismo mensaje me permite cancelar o reagendar.
  - Trazabilidad: R-04 · Dolores: `olvido-cita`, `espacios-perdidos-no-show`, `impacto-no-shows`
  - Fuente: `paciente.md`, `secretaria.md`, `doctor.md`

- **[US-05]** Como **paciente**, quiero **cancelar o reagendar mi cita desde un enlace** para **no tener que llamar ni escribir y esperar, liberando el espacio para alguien más**.
  - Criterios de aceptación:
    - Dado que tengo una cita confirmada, cuando uso el enlace para cancelar, entonces el turno vuelve a quedar disponible para otros pacientes.
    - Dado que quiero reagendar, cuando elijo un nuevo turno libre, entonces el turno anterior se libera y la cita queda en el nuevo horario sin intervención de recepción.
  - Trazabilidad: R-05 · Dolores: `reagendamiento-laborioso`, `espacios-perdidos-no-show`
  - Fuente: `paciente.md`, `secretaria.md`

### Secretaria

- **[US-06]** Como **secretaria**, quiero **que el sistema impida agendar dos pacientes en el mismo turno de un doctor** para **eliminar el doble agendamiento y los horarios mal anotados**.
  - Criterios de aceptación:
    - Dado un turno ya ocupado, cuando alguien (paciente o recepción) intenta reservarlo, entonces el sistema rechaza la operación y muestra que el turno ya no está libre.
    - Dado que dos reservas llegan casi a la vez para el mismo turno, cuando se procesan, entonces solo una queda registrada y la otra es rechazada.
  - Trazabilidad: R-06 · Dolores: `doble-agendamiento`, `multiples-canales-sin-fuente-verdad`
  - Fuente: `secretaria.md`

- **[US-07]** Como **secretaria**, quiero **registrar y actualizar el estado de cada cita en un solo lugar** para **tener una única fuente de verdad y dejar de cruzar llamadas, WhatsApp y agenda**.
  - Criterios de aceptación:
    - Dado cualquier turno, cuando gestiono la cita, entonces puedo marcarla como `pendiente`, `confirmado`, `cancelado`, `atendido` o `no asistió`.
    - Dado que cambio el estado de una cita, cuando el doctor consulta su agenda, entonces ve el estado actualizado al momento.
    - Dado un paciente que llega, cuando lo busco, entonces encuentro su cita en el sistema sin revisar mensajes ni llamadas.
  - Trazabilidad: R-07, R-15 · Dolores: `multiples-canales-sin-fuente-verdad`, `confirmacion-manual`
  - Fuente: `secretaria.md`

### Doctor

- **[US-08]** Como **doctor**, quiero **ver mi agenda del día en tiempo real con el contexto de cada cita** para **llegar a cada turno informado y no atender a ciegas ni sorprenderme con los cambios**.
  - Criterios de aceptación:
    - Dado mi día de atención, cuando abro mi agenda, entonces veo cada cita con nombre del paciente, tipo de consulta (primera vez / control), motivo, hora asignada y estado de confirmación.
    - Dado que recepción o un paciente cambia una cita, cuando consulto la agenda, entonces el cambio se refleja sin que nadie me avise aparte.
  - Trazabilidad: R-09, R-10 · Dolores: `sin-contexto-previo`, `sin-visibilidad-tiempo-real`, `agenda-impredecible`
  - Fuente: `doctor.md`

---

## Fuera del núcleo (backlog, no entran al MVP)

Estas historias tienen respaldo en evidencia pero **no atacan el núcleo de valor**;
se postergan para no inflar el MVP (ver `mvp-canvas.md` → *Fuera de alcance*).

- **[US-09]** Como **doctor**, quiero **bloquear rangos de mi agenda** (reuniones, procedimientos, emergencias) para que no se ofrezcan esos turnos. · R-08 · Fuente: `doctor.md`
- **[US-10]** Como **secretaria**, quiero **un reporte diario por doctor** (total, confirmadas, canceladas, no asistidas) para rendir cuentas a gestión. · R-11 · Fuente: `secretaria.md`
- **[US-11]** Como **secretaria**, quiero **identificar los horarios con mayor ausentismo** para reorganizar la agenda. · R-12 · Fuente: `secretaria.md`

> Estas se construyen **después** de validar que el núcleo (reserva autónoma +
> recordatorio + fuente única) realmente reduce no-shows y desorden. Adelantarlas
> es optimizar algo que aún no sabemos si la gente usará.
