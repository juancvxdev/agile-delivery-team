# ADR-0001 · Una sola aplicación con persistencia transaccional como fuente única de verdad
**Estado:** aceptado
**Fecha:** 2026-06-20

## Contexto y fuerza
El núcleo del MVP es reemplazar el caos de canales (llamada/WhatsApp/papel) por
**una única fuente de verdad** donde paciente, recepción y doctor ven el mismo
estado al momento (`mvp-canvas.md` → propuesta de valor; `pain:multiples-canales-sin-fuente-verdad`).
La épica **E-03** existe precisamente para que recepción opere sobre esa fuente
única (`mvp:fuente-unica-estado`, **req:R-07**), y **US-07** exige registrar el
estado de cada cita "en un solo lugar". Además **R-17** y **US-01** exigen que la
disponibilidad mostrada refleje el estado real en tiempo real.

La fuerza de fondo es de simplicidad: el equipo planifica el delivery de un **MVP**
cuyo mayor riesgo es la **adopción de recepción** (`riesgo-abandono-sistema`,
`mvp-canvas.md` riesgo 1). Toda complejidad que no se necesite hoy conspira contra
esa adopción y contra el tiempo de entrega.

## Decisión
El MVP se construye como **una sola aplicación (monolito modular) respaldada por
una base de datos relacional con transacciones ACID** que es la **única fuente de
verdad** del estado de turnos y citas. Toda lectura de disponibilidad y toda
escritura de citas pasa por esa base; no hay copias de la agenda en otros canales.
La aplicación expone las tres vistas (reserva del paciente, recepción, agenda del
doctor) sobre el mismo modelo de datos.

## Alternativas consideradas
- **Microservicios / arquitectura distribuida desde el día uno** — añade
  coordinación, consistencia eventual entre servicios y operación compleja que el
  MVP no necesita; trabaja en contra de la simplicidad y de R-17 (consistencia
  inmediata). Es trabajo no hecho que debe seguir sin hacerse hasta tener tracción.
- **Almacenamiento NoSQL sin transacciones multi-documento fuertes** — complica
  garantizar el anti doble-agendamiento (**R-06**) con una sola escritura atómica;
  ver ADR-0002.
- **Mantener canales actuales sincronizados (WhatsApp/papel + sistema)** —
  contradice la propuesta de valor de fuente única y reintroduce el dolor que el
  MVP ataca (`mvp-canvas.md` → fuera de alcance: "Mantener WhatsApp vivo en
  paralelo conspira contra la fuente única").

## Consecuencias
- **Ganamos:** una única fuente de verdad transaccional que sostiene E-03, R-07,
  R-17 y habilita el anti doble-agendamiento (ADR-0002) con el mínimo de piezas
  móviles; menor superficie operativa, despliegue y razonamiento más simples,
  coherente con el riesgo de adopción.
- **Costo aceptado:** la disponibilidad continua en hora pico (**R-16**) depende de
  un solo despliegue; se mitiga con medidas operativas simples (ver ADR-0003 y la
  open question sobre escalado). El escalado horizontal del cómputo y la
  separación en servicios se difieren hasta que la adopción lo justifique.
