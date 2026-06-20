# Historias refinadas — citasdentista

> Refinadas por el **Developer** del Agile Delivery Team a partir de `backlog.json`
> y el `inbox/` (mvp-canvas, user-stories, requisitos, personas, evidence-map).
>
> **Estas historias ya pasaron el gate `dor-invest-gate` (DoR + INVEST):** cada una
> tiene `Como/quiero/para` completo, criterios de aceptación verificables en Gherkin,
> estimación en puntos (≤ 8) y **cero preguntas abiertas bloqueantes**.
>
> **Cero invención:** cada criterio traza al descubrimiento vía `Origen`. Las antiguas
> preguntas abiertas (US-02, US-03, US-04, US-05, US-06) NO se respondieron inventando
> un hecho del inbox: se resolvieron como **decisión de refinamiento del equipo** (la
> "N" de INVEST), mínima y consistente con el MVP, y se aterrizaron en un criterio
> testeable. Esas decisiones quedan marcadas con "Decisión de refinamiento:" y el
> equipo puede revisarlas en el siguiente refinamiento.

---

## E-01 · El paciente reserva su cita solo, viendo turnos reales

### US-01 · Ver disponibilidad real · épica E-01 · 5 pts
**Como** paciente, **quiero** ver los horarios realmente disponibles por doctor y día, **para** elegir uno sin tener que preguntar uno a uno y esperar respuesta.

Criterios de aceptación (Gherkin):
- Dado que abro el enlace de reserva, cuando selecciono un doctor y un día, entonces veo solo los turnos libres de ese doctor para ese día.
- Dado que un turno acaba de ser ocupado por otra persona, cuando consulto la disponibilidad, entonces ese turno ya no aparece como libre.
- Dado que no hay turnos libres en el día elegido, cuando lo selecciono, entonces el sistema me lo indica y me ofrece el siguiente día con cupo.

Origen: us:US-01 · req:R-01 · req:R-17 · pain:sin-visibilidad-horarios · pain:desconfianza-disponibilidad

---

### US-02 · Reservar con datos propios · épica E-01 · 5 pts
**Como** paciente, **quiero** reservar mi cita desde un enlace ingresando yo mismo mis datos (nombre, cédula, teléfono, correo, motivo), **para** no depender de un chat largo con recepción y agendar en pocos pasos desde el celular.

Criterios de aceptación (Gherkin):
- Dado que elegí doctor, horario y motivo, cuando completo nombre, cédula, teléfono y correo, entonces el sistema crea la cita en estado `pendiente` y la asocia a ese turno.
- Dado que envío la reserva, cuando recepción la revisa, entonces los datos ya están capturados y solo los valida (no los recoge de nuevo).
- Dado que es una cita de control y no primera vez, cuando reservo, entonces el sistema no me exige más datos de los necesarios.
- Dado que mi cita queda en estado `pendiente`, cuando transcurren 24 horas sin que recepción la valide, entonces el sistema libera automáticamente el turno y la cita pasa a estado `expirado`.
- Dado que mi cita pendiente sigue dentro de la ventana de 24 horas, cuando otro paciente consulta ese mismo turno, entonces el turno aparece como ocupado y no puede reservarse.

**Decisión de refinamiento:** el inbox no define cuánto retiene el turno una cita en `pendiente` ni qué pasa si recepción no la valida. El equipo fija una **ventana de retención de 24 h**: pasado ese plazo sin validación, el turno se libera automáticamente y la cita se marca `expirado`. Es una decisión negociable (revisable en refinamiento), no un hecho del Discovery.

Origen: us:US-02 · req:R-02 · req:R-13 · req:R-14 · pain:sin-visibilidad-horarios · pain:recopilacion-datos-repetitiva

---

## E-02 · El paciente confirma, recuerda y libera su cita (menos no-shows)

### US-03 · Comprobante de confirmación · épica E-02 · 3 pts
**Como** paciente, **quiero** recibir un comprobante de confirmación por correo electrónico apenas reservo, **para** quedar seguro de que la cita quedó agendada y no con la duda.

Criterios de aceptación (Gherkin):
- Dado que completo la reserva con un correo válido, cuando el sistema la registra, entonces recibo de inmediato por correo electrónico un comprobante con doctor, fecha, hora y motivo.
- Dado que recibí el comprobante, cuando llego a la clínica, entonces mi cita aparece en el sistema con los mismos datos del comprobante.
- Dado que el envío del comprobante falla, cuando consulto el enlace de la reserva, entonces puedo ver el comprobante en pantalla y reenviarlo a mi correo.

**Decisión de refinamiento:** el inbox dice "comprobante" pero no fija el canal. El equipo elige **correo electrónico** como canal por defecto del MVP (R-13 ya captura el correo del paciente). El canal podría hacerse configurable más adelante; el criterio queda testeable sobre correo.

Origen: us:US-03 · req:R-03 · pain:incertidumbre-confirmacion · pain:desconfianza-disponibilidad

---

### US-04 · Recordatorio automático · épica E-02 · 5 pts
**Como** paciente, **quiero** recibir un recordatorio automático por correo electrónico antes de la cita, **para** no olvidarla y poder cancelar a tiempo si no podré asistir.

Criterios de aceptación (Gherkin):
- Dado que tengo una cita confirmada, cuando faltan 24 horas para la cita, entonces recibo por correo electrónico un recordatorio con fecha, hora, doctor e instrucciones previas.
- Dado que recibo el recordatorio, cuando no podré asistir, entonces el mismo mensaje incluye el enlace para cancelar o reagendar la cita.
- Dado que la cita se reservó con menos de 24 horas de anticipación, cuando se confirma, entonces el recordatorio se envía de inmediato tras la confirmación.

**Decisión de refinamiento:** el inbox no fija la anticipación, el número de envíos ni el canal. El equipo define **un único envío, 24 h antes, por correo electrónico** (coherente con US-03). Si la cita queda dentro de las 24 h, el recordatorio sale al confirmar. Decisión negociable.

Origen: us:US-04 · req:R-04 · pain:olvido-cita · pain:espacios-perdidos-no-show · pain:impacto-no-shows

---

### US-05 · Cancelar / reagendar por enlace · épica E-02 · 5 pts
**Como** paciente, **quiero** cancelar o reagendar mi cita desde un enlace único y personal, **para** no tener que llamar ni escribir y esperar, liberando el espacio para alguien más.

Criterios de aceptación (Gherkin):
- Dado que tengo una cita confirmada, cuando uso el enlace para cancelar, entonces el turno vuelve a quedar disponible para otros pacientes.
- Dado que quiero reagendar, cuando elijo un nuevo turno libre, entonces el turno anterior se libera y la cita queda en el nuevo horario sin intervención de recepción.
- Dado que el enlace de gestión es único, personal y no adivinable (token aleatorio), cuando se abre, entonces solo permite gestionar la cita asociada a ese enlace y ninguna otra.
- Dado un enlace de gestión alterado o inválido, cuando alguien lo abre, entonces el sistema niega el acceso y no muestra datos de ninguna cita.

**Decisión de refinamiento:** el inbox habla de un "enlace" pero no define cómo se evita que un tercero acceda a la cita. El equipo define que el enlace lleva un **token único, personal y no adivinable**, con alcance limitado a su propia cita. Decisión negociable que la Arquitectura detallará.

Origen: us:US-05 · req:R-05 · pain:reagendamiento-laborioso · pain:espacios-perdidos-no-show

---

## E-03 · Recepción opera sobre una única fuente de verdad, sin choques

### US-06 · Anti doble-agendamiento · épica E-03 · 5 pts
**Como** secretaria, **quiero** que el sistema impida agendar dos pacientes en el mismo turno de un doctor, **para** eliminar el doble agendamiento y los horarios mal anotados.

Criterios de aceptación (Gherkin):
- Dado un turno ya ocupado, cuando alguien (paciente o recepción) intenta reservarlo, entonces el sistema rechaza la operación y muestra que el turno ya no está libre.
- Dado que dos reservas llegan casi a la vez para el mismo turno, cuando se procesan, entonces solo la primera en confirmarse (primer commit gana, por orden de llegada) queda registrada y la otra es rechazada con un mensaje que ofrece elegir otro turno.
- Dado que mi reserva fue rechazada por concurrencia, cuando vuelvo a la disponibilidad, entonces ese turno ya no aparece como libre.

**Decisión de refinamiento:** R-06 exige que solo una de dos reservas casi simultáneas quede registrada, pero el inbox no fija la regla de desempate. El equipo adopta **primer commit gana (orden de llegada)**. Decisión negociable; la Arquitectura definirá el mecanismo (p. ej. restricción de unicidad / bloqueo).

Origen: us:US-06 · req:R-06 · pain:doble-agendamiento · pain:multiples-canales-sin-fuente-verdad

---

### US-07 · Estado de cita en un solo lugar · épica E-03 · 5 pts
**Como** secretaria, **quiero** registrar y actualizar el estado de cada cita (pendiente, confirmado, cancelado, atendido, no asistió) en un solo lugar, **para** tener una única fuente de verdad y dejar de cruzar llamadas, WhatsApp y agenda.

Criterios de aceptación (Gherkin):
- Dado cualquier turno, cuando gestiono la cita, entonces puedo marcarla como `pendiente`, `confirmado`, `cancelado`, `atendido` o `no asistió`.
- Dado que cambio el estado de una cita, cuando el doctor consulta su agenda, entonces ve el estado actualizado al momento.
- Dado un paciente que llega, cuando lo busco, entonces encuentro su cita en el sistema sin revisar mensajes ni llamadas.

Origen: us:US-07 · req:R-07 · req:R-15 · pain:multiples-canales-sin-fuente-verdad · pain:confirmacion-manual

---

## E-04 · El doctor llega a cada turno informado y con agenda al día

### US-08 · Agenda del doctor en tiempo real · épica E-04 · 5 pts
**Como** doctor, **quiero** ver mi agenda del día en tiempo real con el contexto de cada cita (nombre, tipo de consulta, motivo, hora y estado), **para** llegar a cada turno informado y no atender a ciegas ni sorprenderme con los cambios.

Criterios de aceptación (Gherkin):
- Dado mi día de atención, cuando abro mi agenda, entonces veo cada cita con nombre del paciente, tipo de consulta (primera vez / control), motivo, hora asignada y estado de confirmación.
- Dado que recepción o un paciente cambia una cita, cuando consulto la agenda, entonces el cambio se refleja sin que nadie me avise aparte.

Origen: us:US-08 · req:R-09 · req:R-10 · pain:sin-contexto-previo · pain:sin-visibilidad-tiempo-real · pain:agenda-impredecible

---

## Resumen

| Historia | Pts | Épica | Prioridad |
|----------|-----|-------|-----------|
| US-01 · Ver disponibilidad real        | 5 | E-01 | 1 |
| US-02 · Reservar con datos propios      | 5 | E-01 | 2 |
| US-03 · Comprobante de confirmación     | 3 | E-02 | 3 |
| US-04 · Recordatorio automático         | 5 | E-02 | 4 |
| US-05 · Cancelar / reagendar por enlace | 5 | E-02 | 5 |
| US-06 · Anti doble-agendamiento         | 5 | E-03 | 6 |
| US-07 · Estado de cita en un solo lugar | 5 | E-03 | 7 |
| US-08 · Agenda del doctor en tiempo real| 5 | E-04 | 8 |

**Total: 8 historias · 38 puntos.**

> Todas cumplen DoR/INVEST y `open_questions: []`. Dependencias (todas a IDs
> existentes): US-02→US-01; US-03→US-02; US-04→US-03, US-05; US-05→US-02;
> US-06→US-02; US-07→US-02; US-08→US-07.
