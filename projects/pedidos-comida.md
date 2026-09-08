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

### Cadena de arranque (tras reinicio de Mac)

login → colima (LaunchAgent) → contenedor (restart policy) → túnel
(LaunchAgent, re-registra botón 🍽 con URL nueva) → bot operativo.
Todo automático, verificado con simulación stop/start.

## Runtime

- **Arranque:** automático en cascada (ver arriba)
- **Detener:** `docker compose down` (y `launchctl unload` de los LaunchAgents si es total)
- **Verificar salud:** `docker compose ps` + `docker compose logs -f pedidos-bot` + botón 🍽 abre la Web App

## Pendientes

- [ ] Prueba del ciclo real: pedido → corte viernes 17:00 → reporte
- [ ] Migrar de quick tunnel (URL aleatoria) a túnel nombrado de Cloudflare con dominio propio
- [ ] Definir menú real de la semana y agregar personas al grupo
- [ ] Reporte del sábado (hoy el corte envía el resumen el viernes 17:00; evaluar reenvío sábado por la mañana)

## Bitácora de decisiones

| Fecha | Decisión |
|---|---|
| 2026-09-07 | Alta del proyecto; bot chat-native primero (referencia Durger King es chat-native) |
| 2026-09-07 | SQLite sobre PostgreSQL: cero setup para grupo pequeño; migrable |
| 2026-09-07 | Review independiente: guard is_week_closed en on_confirm, config muerta removida, fix main.py committeado |
| 2026-09-07 | Docker + colima para desacoplar del ambiente local; launchd del bot reemplazado por contenedor |
| 2026-09-07 | Incidente: token perdido al reescribir .env (scrubber enmascaró mi vista); restaurado por el usuario desde BotFather |
| 2026-09-07 | Menú '/' no aparecía por cache del cliente → Web App con botón de menú (setChatMenuButton) como vía principal |
| 2026-09-07 | Túnel cloudflared quick con auto-re-registro del botón; DNS del router no resuelve *.trycloudflare.com (Telegram sí) |
| 2026-09-07 | API de la Web App autenticada con initData HMAC — nadie puede pedir por otro |
| 2026-09-07 | Producción verificada: Web App 200 vía túnel, botón propagado, polling activo |
