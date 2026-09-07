# iot-water-flow

| Campo | Valor |
|---|---|
| Estado | active (PoC completado, producción pendiente) |
| Dueño | atodamadresiempre |
| Creado | 2026 (PoC) |
| Última revisión | 2026-09-07 |

## Descripción

PoC de monitoreo de flujo de agua con dashboard en tiempo real. Sensores
publican por MQTT; Node-RED procesa y sirve el dashboard web.

## Stack

- Node-RED v5 + @flowfuse/node-red-dashboard 2.0
- Mosquitto (broker MQTT, Homebrew) — config en /opt/homebrew/etc/mosquitto/mosquitto.conf
- Node.js v24 (usar /usr/local/bin/node)

## Rutas locales

| Qué | Ruta |
|---|---|
| Código fuente / flows | ~/iot-water-flow/ |
| Node-RED user dir | ~/iot-water-flow/nr-fresh |
| Config Mosquitto | /opt/homebrew/etc/mosquitto/mosquitto.conf |

## Runtime

- **Arrancar:** `mosquitto` (brew services) + Node-RED con user dir `nr-fresh`
- **Detener:** brew services stop mosquitto + detener Node-RED
- **Verificar salud:** abrir el dashboard en el navegador

## Notas técnicas

- IDs de nodos en el flow JSON deben estar en formato hex.
- Dashboard en español (estático, sin i18n en runtime — decisión del usuario).

## Pendientes

- [ ] Instanciar en producción
- [ ] Definir ambiente destino (¿Raspberry/local server?)

## Bitácora de decisiones

| Fecha | Decisión |
|---|---|
| 2026-09 | Español estático sobre i18n runtime: menos complejidad, sin pérdidas de estado |
| 2026-09 | Node IDs hex obligatorios en flows (corrigió errores de import) |