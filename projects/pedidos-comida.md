# pedidos-comida

| Campo | Valor |
|---|---|
| Estado | active (en producción) |
| Dueño | atodamadresiempre |
| Creado | 2026-09-07 |
| Última revisión | 2026-09-07 |

## Descripción

Mini-app de Telegram para pedidos de comida durante la semana: corte
viernes 17:00, resumen total para preparar y surtir el sábado.
Referencia UX: Durger King Bot.

## Stack

- Python 3.11 + python-telegram-bot v21 (bot + conversación /menu)
- Web App (aiohttp :8080): mini página estilo Durger King — botón de
  menú junto al textbox (setChatMenuButton), stepper, notas, borrado
- APScheduler (corte vie 17:00 + recordatorio jue 18:00)
- SQLite (convención ct_/st_) en volumen Docker persistente
- Docker (colima) + túnel cloudflared (HTTPS público para la Web App)
- Repo: github.com/atodamadresiempre/pedidos-comida (privado)
- Bot: @Salsabrozas_bot

## Rutas y despliegue

### Rutas locales

| Qué | Dónde |
|---|---|
| Código | ~/pedidos-comida/ |
| Contenedor | docker compose (pedidos-bot, restart unless-stopped) |
| DB | volumen Docker pedidos-comida_pedidos-data:/data/pedidos.db |
| Túnel público | LaunchAgent com.local.webapp-tunnel (auto-re-registra URL en el bot) |
| Colima al login | LaunchAgent com.local.colima.start |

Arranque total tras reinicio de Mac: login → colima → contenedor →
túnel → botón actualizado. Todo automático.

## Runtime

- **Arranque:** automático en cascada — login → colima (LaunchAgent) → contenedor (restart policy) → túnel (LaunchAgent) → botón actualizado
- **Detener:** `docker compose down` (y `launchctl unload` de los LaunchAgents si es total)
- **Verificar salud:** `docker compose ps` + `docker compose logs -f pedidos-bot` + botón 🍽 abre la Web App

## Operación

- Logs: `docker compose logs -f pedidos-bot`
- Reconstruir: `docker compose up -d --build`
- Menú: editar ct_menu_items en la DB (o static/menu_seed.sql + recrear volumen)
- Token: en .env (fuera de git), formato docker compose env_file

## Pendientes

- [ ] Prueba del ciclo real: pedido → corte viernes → reporte sábado
- [ ] Migrar de quick tunnel (URL aleatoria) a dominio propio con túnel nombrado de Cloudflare (opcional)
- [ ] Agregar personas al grupo y ajustar el menú real

## Bitácora de decisiones

| Fecha | Decisión |
|---|---|
| 2026-09-07 | Bot chat-native primero; Web App solo cuando el menú '/' no apareció por cache del cliente |
| 2026-09-07 | SQLite sobre PostgreSQL: cero setup para grupo pequeño; migrable |
| 2026-09-07 | Docker + colima para desacoplar del ambiente local; LaunchAgents para auto-arranque |
| 2026-09-07 | Túnel cloudflared quick con auto-re-registro del botón (los quick tunnels cambian URL) |
| 2026-09-07 | API de la Web App autenticada con initData HMAC — nadie puede pedir por otro |
