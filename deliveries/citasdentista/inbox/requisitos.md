# Requisitos candidatos — citasdentista

> Generado desde: `doctor.md`, `paciente.md`, `secretaria.md`
> Todo requisito cita la entrevista y la persona que lo origina.

---

## Funcionales

- **[R-01]** El sistema debe mostrar los horarios disponibles en tiempo real, agrupados por doctor y por día.
  - Tipo: funcional
  - Origen: paciente.md · Paciente; secretaria.md · Secretaria

- **[R-02]** El sistema debe permitir al paciente reservar una cita seleccionando doctor, horario disponible y motivo de consulta.
  - Tipo: funcional
  - Origen: paciente.md · Paciente

- **[R-03]** El sistema debe enviar al paciente una confirmación automática (comprobante) inmediatamente después de reservar.
  - Tipo: funcional
  - Origen: paciente.md · Paciente

- **[R-04]** El sistema debe enviar recordatorios automáticos al paciente antes de la cita (fecha, hora, doctor, instrucciones previas).
  - Tipo: funcional
  - Origen: paciente.md · Paciente; secretaria.md · Secretaria

- **[R-05]** El sistema debe permitir al paciente cancelar o reagendar su cita desde un enlace, sin necesidad de llamar a recepción.
  - Tipo: funcional
  - Origen: paciente.md · Paciente; secretaria.md · Secretaria

- **[R-06]** El sistema debe impedir agendar dos pacientes en el mismo turno del mismo doctor (validación de conflicto de horario).
  - Tipo: funcional
  - Origen: secretaria.md · Secretaria

- **[R-07]** El sistema debe permitir registrar y actualizar el estado de cada cita: pendiente, confirmado, cancelado, atendido, no asistió.
  - Tipo: funcional
  - Origen: secretaria.md · Secretaria

- **[R-08]** El sistema debe permitir al doctor bloquear rangos de horario en su agenda (reuniones, procedimientos, emergencias).
  - Tipo: funcional
  - Origen: doctor.md · Doctor

- **[R-09]** El doctor debe poder consultar su agenda en tiempo real, con el estado de confirmación actualizado de cada cita.
  - Tipo: funcional
  - Origen: doctor.md · Doctor

- **[R-10]** La vista de agenda del doctor debe mostrar por cada cita: nombre del paciente, tipo de consulta (primera vez / control), motivo, hora asignada y estado.
  - Tipo: funcional
  - Origen: doctor.md · Doctor

- **[R-11]** El sistema debe generar un reporte diario por doctor: total de citas, confirmadas, canceladas y no asistidas.
  - Tipo: funcional
  - Origen: secretaria.md · Secretaria

- **[R-12]** El sistema debe identificar y reportar los horarios con mayor tasa de ausentismo.
  - Tipo: funcional
  - Origen: secretaria.md · Secretaria

- **[R-13]** El sistema debe generar un enlace de reserva que el paciente complete de forma autónoma (nombre, cédula, teléfono, correo, motivo), reduciendo la intervención de recepción.
  - Tipo: funcional
  - Origen: secretaria.md · Secretaria; paciente.md · Paciente

---

## No funcionales

- **[R-14]** El flujo de reserva para el paciente debe completarse en el menor número de pasos posible y ser completamente operable desde un dispositivo móvil.
  - Tipo: no funcional — usabilidad
  - Origen: paciente.md · Paciente

- **[R-15]** La interfaz de recepción debe ser rápida de operar: agendar o modificar una cita sin abrir múltiples pantallas ni requerir tiempo extra en hora pico.
  - Tipo: no funcional — usabilidad operativa
  - Origen: secretaria.md · Secretaria; doctor.md · Doctor

- **[R-16]** El sistema debe mantener disponibilidad continua en hora pico sin caídas que interrumpan la operación de recepción.
  - Tipo: no funcional — disponibilidad
  - Origen: secretaria.md · Secretaria

- **[R-17]** Los horarios mostrados al paciente deben reflejar el estado real de disponibilidad en tiempo real (sin mostrar turnos ya ocupados).
  - Tipo: no funcional — consistencia de datos
  - Origen: paciente.md · Paciente; secretaria.md · Secretaria
