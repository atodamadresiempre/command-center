# pedidos-comida

| Campo | Valor |
|---|---|
| Estado | active (MVP en desarrollo) |
| Dueño | atodamadresiempre |
| Creado | 2026-09-07 |
| Última revisión | 2026-09-07 |

## Descripción

Mini-app de Telegram para pedidos de comida durante la semana: corte
viernes 17:00, resumen total para preparar y surtir el sábado.
Referencia UX: Durger King Bot.

## Stack

- Python 3.11 + python-telegram-bot v21 (bot puro, botones inline)
- APScheduler (corte y recordatorio en el mismo proceso)
- SQLite (convención ct_/st_)
- Repo: github.com/atodamadresiempre/pedidos-comida (privado)

## Rutas locales

| Qué | Ruta |
|---|---|
| Código fuente | ~/pedidos-comida/ |
| Base de datos | ~/pedidos-comida/data/pedidos.db |
| venv | ~/pedidos-comida/.venv |

## Runtime

- **Arrancar:** `./.venv/bin/python -m src.main` (envía PEDIDOS_BOT_TOKEN, PEDIDOS_HOME_CHAT_ID, PEDIDOS_ADMIN_IDS)
- **Detener:** SIGINT del proceso
- **Verificar salud:** el bot responde /start en Telegram; logs en stdout

## Despliegue

Proceso local en el Mac (futuro: launchd service o Raspberry).

## Pendientes

- [ ] Crear bot real en @BotFather + configurar env vars
- [ ] Probar conversación end-to-end en Telegram real
- [ ] Definir menú real de la semana
- [ ] Desplegar como servicio launchd (auto-arranque)

## Bitácora de decisiones

| Fecha | Decisión |
|---|---|
| 2026-09-07 | Bot de chat puro sobre Web App: la referencia Durger King es chat-native y el MVP no necesita web |
| 2026-09-07 | SQLite sobre PostgreSQL: cero setup para un grupo pequeño; migrable |
| 2026-09-07 | APScheduler en el mismo proceso del bot: una sola pieza operativa, sin cron externo |