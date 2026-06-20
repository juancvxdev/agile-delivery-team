# ADR-0005 · Acceso del paciente por enlace con token único no adivinable (sin login)
**Estado:** aceptado
**Fecha:** 2026-06-20

## Contexto y fuerza
**R-05** y **US-05** exigen que el paciente pueda **cancelar o reagendar su cita
desde un enlace, sin llamar a recepción y sin crear cuenta**. El refinamiento de
US-05 ya decidió que el enlace lleva un **token aleatorio, único, personal y no
adivinable**, que su alcance se **limita a la cita asociada** y que un **enlace
alterado o inválido es rechazado sin mostrar datos de ninguna cita**. La fuerza de
usabilidad es **R-14** (flujo móvil, en los menos pasos posibles): pedir registro o
contraseña añadiría fricción que el paciente no tolera (`reagendamiento-laborioso`).
El mismo enlace es donde se ve y reenvía el comprobante (US-03) y desde donde el
recordatorio permite cancelar (US-04).

## Decisión
El acceso del paciente a su cita (ver comprobante, cancelar, reagendar) se autoriza
mediante un **token de capacidad**: una cadena **aleatoria de alta entropía, no
adivinable**, generada al crear la cita y asociada **únicamente a esa cita**. El
enlace que se envía por correo (comprobante US-03, recordatorio US-04) contiene ese
token. Al abrirlo, el sistema **valida el token y solo expone/gestiona la cita
asociada**; ante token ausente, alterado o inexistente **niega el acceso sin revelar
información** de ninguna cita. **No hay login ni contraseña** para el paciente.

## Alternativas consideradas
- **Login con usuario/contraseña o registro** — añade fricción y un paso que R-14 y
  el dolor `reagendamiento-laborioso` piden evitar; sobredimensiona el MVP.
- **Identificador secuencial/predecible en la URL** — sería adivinable y permitiría
  acceder a citas ajenas, violando el criterio de US-05 de "no adivinable" y de
  negar enlaces inválidos.
- **Verificación por código OTP enviado en cada gestión** — más seguro pero más
  pasos; no está pedido por el inbox y choca con R-14. Queda como posible refuerzo
  futuro (open question).

## Consecuencias
- **Ganamos:** US-05 cumplida con cero fricción de autenticación, coherente con R-14;
  reutiliza el mismo enlace para comprobante y recordatorio; rechazo seguro de
  enlaces inválidos.
- **Costo aceptado:** quien posea el enlace puede gestionar la cita (modelo de
  capacidad); el riesgo se acota porque el token solo da acceso a **una** cita y no
  contiene datos sensibles más allá de esa cita. Caducidad/rotación del token tras
  uso, o un segundo factor, no están pedidos por el inbox y se difieren como open
  question.
