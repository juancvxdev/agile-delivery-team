# ADR-0003 · Notificaciones por correo asíncronas, con reintentos y recordatorio programado
**Estado:** aceptado
**Fecha:** 2026-06-20

## Contexto y fuerza
La **métrica de éxito del MVP es la tasa de no-show** y la épica **E-02** la mueve
con dos mecanismos de notificación: **US-03** (comprobante de confirmación
inmediato) y **US-04** (recordatorio 24 h antes). Ambas historias ya fijaron
**correo electrónico como canal por defecto del MVP**. **US-04** exige envío
**programado** ("cuando faltan 24 horas") y, si la cita se reservó con menos de 24 h
de anticipación, envío **inmediato tras la confirmación**. **US-03** contempla
explícitamente el **fallo de envío** ("Dado que el envío del comprobante falla...").
La fuerza adicional es de disponibilidad y adopción: el envío de correo no debe
bloquear ni frenar la reserva en hora pico (**R-15**, **R-16**).

## Decisión
Las notificaciones se envían de forma **asíncrona** mediante un componente
**Servicio de Notificaciones** que entrega correo a través de un proveedor de
correo externo. La transacción de reserva **encola** el comprobante y **no espera**
al envío (no bloquea al paciente). El **recordatorio se programa** vía un
**Programador (scheduler)** que cada cierto intervalo selecciona las citas
`confirmadas` cuya hora está a ~24 h y dispara su envío; si la cita se confirma con
menos de 24 h, el recordatorio se encola de inmediato. Ante **fallo de envío** se
aplican **reintentos** con backoff; el comprobante queda además **visible en el
enlace de la reserva** y **reenviable** desde ahí (criterio de US-03), de modo que
un fallo de correo nunca deja al paciente sin comprobante.

## Alternativas consideradas
- **Envío síncrono dentro de la transacción de reserva** — acopla la reserva a la
  latencia/disponibilidad del proveedor de correo; un correo lento o caído frenaría
  la operación en hora pico, en contra de R-15/R-16.
- **SMS / WhatsApp como canal del MVP** — las historias fijaron correo como canal
  por defecto; añadir otro canal ahora es alcance no respaldado (queda como open
  question del backlog: "canal de confirmación/recordatorio").
- **Cron externo manual / envío disparado a mano por recepción** — reintroduce
  trabajo manual (`confirmacion-manual`) que el MVP busca eliminar.

## Consecuencias
- **Ganamos:** US-03 y US-04 cumplidas sin acoplar la reserva al correo; resiliencia
  ante fallos del proveedor (reintentos + comprobante en pantalla); la reserva sigue
  siendo rápida en hora pico.
- **Costo aceptado:** se introducen dos piezas (cola/worker de envío y scheduler)
  que el monolito puede alojar internamente para no sumar infraestructura. La
  política exacta de número de reintentos, la frecuencia del scheduler y el uso de
  un proveedor concreto de correo se afinan en implementación (open question). Un
  único envío de recordatorio 24 h antes es lo decidido en US-04; cadencias
  adicionales quedan fuera del MVP.
