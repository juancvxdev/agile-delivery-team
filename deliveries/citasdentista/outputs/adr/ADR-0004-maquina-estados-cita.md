# ADR-0004 · Máquina de estados de la cita como fuente de verdad, con expiración del pendiente a las 24 h
**Estado:** aceptado
**Fecha:** 2026-06-20

## Contexto y fuerza
**R-07** y **US-07** exigen registrar y actualizar el estado de cada cita
(**pendiente, confirmado, cancelado, atendido, no asistió**) en un solo lugar, y que
ese estado sea el que ven recepción y el doctor al momento (**US-08**, **R-09/R-10**).
La métrica de éxito (tasa de no-show) se calcula como citas `no asistió` ÷ citas
`confirmadas` (`mvp-canvas.md`), por lo que **los estados deben ser explícitos y
consistentes** o la métrica no se puede medir. Además **US-02** fijó que una cita
nace en `pendiente` y, si recepción no la valida, **expira a las 24 h** liberando el
turno; mientras está pendiente dentro de la ventana, el turno aparece ocupado.

## Decisión
Se modela la cita con una **máquina de estados explícita** como única
representación de su ciclo de vida:

```
reservada → pendiente ──(recepción valida)──> confirmado ──> atendido | no asistió
                  │                                  │
                  │ (24 h sin validar → expira)      └──(paciente cancela/reagenda)──> cancelado
                  ▼
              expirado  (libera el turno)
```

Las transiciones válidas se controlan en la aplicación; los estados se persisten en
la base transaccional (ADR-0001). Un **trabajo de expiración** (apoyado en el mismo
Programador de ADR-0003) recorre las citas `pendiente` que superan las **24 h** sin
validación y las pasa a `expirado`, **liberando el turno** al pool de disponibilidad.
Una cita activa (`pendiente`/`confirmado`) ocupa su turno frente a la disponibilidad
(R-17); `cancelado` y `expirado` lo liberan.

## Alternativas consideradas
- **Estados implícitos por banderas booleanas sueltas** — permite combinaciones
  inválidas (p. ej. confirmada y cancelada a la vez), rompe el cálculo de la métrica
  y dificulta auditar quién cambió qué; contradice "una sola fuente de verdad".
- **Sin expiración del pendiente (retención indefinida del turno)** — dejaría turnos
  bloqueados por reservas que recepción nunca valida, contradiciendo US-02 y el
  objetivo de aprovechar turnos.
- **Confirmación automática sin paso de recepción** — recepción quiere **validar**
  los datos capturados (US-02 criterio 2); saltar ese paso le quita el control que
  necesita para adoptar el sistema.

## Consecuencias
- **Ganamos:** una fuente de verdad de estado consistente que sostiene E-03, E-04 y
  el cálculo de la métrica de no-show; la retención de 24 h de US-02 queda
  implementada por el mismo Programador, sin pieza nueva.
- **Costo aceptado:** la ventana de 24 h es una regla de negocio fija del MVP; si en
  campo resulta corta o larga deberá ajustarse (no requiere rediseño). La
  notificación al paciente cuando su cita expira no está pedida por el inbox y queda
  como open question.
