# command-center

Centro de operaciones para el manejo y gobierno de proyectos de software.
Este repositorio es la **fuente de verdad única** para: tooling, plantillas,
playbooks operativos y el registro de proyectos activos.

## Propósito

1. **Registro de proyectos** — cada proyecto vive como una ficha en `projects/`
   con su stack, rutas locales, estado, forma de despliegue y pendientes.
2. **Tooling** — scripts ejecutables en `bin/` y `tools/` para operaciones
   repetitivas (scaffolding, backups, healthchecks).
3. **Plantillas** — bases estandarizadas por tipo de stack en `templates/`.
4. **Conocimiento** — decisiones de arquitectura (ADRs) en `docs/adr/` y
   runbooks paso a paso en `docs/playbooks/`.

## Estructura

```
bin/          Ejecutables CLI del command center
templates/    Plantillas por tipo de stack
tools/        Scripts operativos (backup, healthcheck, deploy)
docs/adr/     Architecture Decision Records
docs/playbooks/  Runbooks: "cómo se hace X paso a paso"
projects/     ★ Registro de proyectos (fichas vivas)
```

## Flujo de trabajo

- Todo cambio entra por **Pull Request** a `main` (1 aprobación, checks en verde).
- Commits en formato **Conventional Commits**: `feat:`, `fix:`, `chore:`, `docs:`.
- Alta de proyecto: abrir issue con la plantilla "Nuevo Proyecto", crear la ficha
  desde `projects/_template.md` y adjuntar el issue al PR.

## Ficha de proyecto (registro)

Cada proyecto registrado en `projects/` documenta:

| Campo | Descripción |
|---|---|
| Estado | active / paused / archived |
| Stack | Tecnologías y versiones |
| Rutas | Path local del código, config, datos |
| Runtime | Cómo se arranca, para y verifica |
| Despliegue | Procedimiento y ambiente destino |
| Dueño | Responsable técnico |
| Pendientes | Backlog corto y verificado |

## Convenciones

- Los nombres de ficha de proyecto usan el nombre del directorio real del
  proyecto en el Mac de desarrollo (ej. `iot-water-flow.md`).
- Los scripts en `bin/` son autocontenidos, ejecutables (`chmod +x`) y con
  `set -euo pipefail`.
- Idioma de documentación: español.

## Origen

Repositorio renombrado desde `-usefullStuff` (2017). El contenido histórico
sin valor corporativo fue retirado; la historia de git se conserva para
trazabilidad.