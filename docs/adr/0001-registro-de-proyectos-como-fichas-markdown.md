# ADR-0001: Registro de proyectos como fichas markdown en el command center

- **Estado:** Aceptado
- **Fecha:** 2026-09-07
- **Decisores:** atodamadresiempre (dueño), Hermes Agent (CTO)

## Contexto

Los proyectos del usuario viven en directorios locales sin un registro
central. El conocimiento (stack, rutas, cómo arrancar, pendientes) está
distribuido en la memoria del agente y en conversaciones pasadas, lo cual
no es persistente ni auditable.

## Decisión

Cada proyecto se registra como una ficha markdown estándar en
`projects/<nombre>.md` dentro del repositorio `command-center`. La ficha
usa la plantilla `projects/_template.md` e incluye: estado, stack, rutas
locales, runtime, despliegue, pendientes y bitácora de decisiones.

## Consecuencias

**Positivas:**
- Fuente de verdad única, versionada y auditable (git).
- Cualquier sesión futura del agente puede leer el registro y operar sin
  re-descubrir el contexto.
- El alta de un proyecto deja rastro (issue + PR).
- La bitácora de decisiones por proyecto crea el hábito del ADR.

**Negativas:**
- Requiere disciplina de actualización (revisión en cada hito).

## Cumplimiento

- Toda ficha nueva usa `_template.md`.
- El nombre del archivo = nombre del directorio real del proyecto.
- Revisión de ficha en cada cierre de hito importante.