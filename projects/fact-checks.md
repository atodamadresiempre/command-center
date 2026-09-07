# fact-checks

| Campo | Valor |
|---|---|
| Estado | active (modelado de datos completado, instancia pendiente) |
| Dueño | atodamadresiempre |
| Creado | 2026 |
| Última revisión | 2026-09-07 |

## Descripción

Sistema de fact-checking: registro estructurado de hechos, actores y sus
participaciones, con dependencias entre hechos modeladas como DAG.

## Stack

- PostgreSQL (instancia pendiente)
- Datasets JSON: formato _headers + _data

## Rutas locales

| Qué | Ruta |
|---|---|
| Código fuente / DDL | ~/fact-checks/ |
| Datasets | ~/fact-checks/ (JSON) |

## Modelo de datos (DDL v1.2, 6 tablas)

Convención de prefijos (estándar corporativo, aplicar a TODOS los proyectos):

- `ct_` = catálogo (pocos cambios: Users, Roles, Products, Categories)
- `rt_` = relacional/lookup (relacionan catálogos: UserRoles, ProductCategories)
- `st_` = transaccional/staging (transacciones o fases de flujo: Sales, SalesDetail)

Tablas:

- `ct_tipo_de_hecho` — catálogo
- `ct_roles` — catálogo
- `ct_actores` — catálogo
- `st_hechos` — transaccional (UUID pk)
- `rt_participa` — relación actores-hechos
- `rt_hecho_dependencias` — DAG N:N entre hechos

## Runtime

- **Arrancar:** pendiente — requiere instancia PostgreSQL
- **Verificar salud:** pendiente

## Pendientes

- [ ] Instanciar en PostgreSQL
- [ ] Cargar datasets JSON
- [ ] Definir API/interfaz de consulta

## Bitácora de decisiones

| Fecha | Decisión |
|---|---|
| 2026-09 | Prefijos ct_/rt_/st_ como estándar para todos los proyectos de BD |
| 2026-09 | Dependencias entre hechos como DAG N:N (rt_hecho_dependencias) |