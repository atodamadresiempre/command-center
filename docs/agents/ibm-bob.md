# Agentes CLI del ecosistema

Registro de agentes de código disponibles para delegación de trabajo.

| Agente | CLI | Uso principal | Estado |
|---|---|---|---|
| IBM Bob | `bob` v2.0.2 | Desarrollo IBM i: RPG, CL, Db2 SQL, ACS | ✔ operativo |
| Hermes | `hermes` | Orquestador principal (este registro) | ✔ operativo |
| Codex | `codex` (~/.npm-global/bin) | Delegación de coding genérico | disponible |

## IBM Bob (bobshell)

- **Binario:** `~/.npm-global/bin/bob` (v2.0.2, npm global)
- **Autenticación:** SSO IBMid almacenado en macOS Keychain ("IBM Bob Safe
  Storage"). Licencia aceptada.
- **Restricción importante:** `bob` lee el keychain SOLO con TTY. Para
  invocarlo desde herramientas sin terminal (scripts, cron, subshells) usar
  PTY (tmux / pty=true) o exportar `BOB_API_KEY` (key de scope Inference
  desde bob.ibm.com → API keys).
- **Endpoint:** api.us-east.bob.ibm.com (gateway IBM Bob)

### Invocación desde Hermes

```bash
# One-shot con prompt (PTY requerido para keychain):
tmux new-session -d -s bob 'bob -p "prompt here"'
sleep 15 && tmux capture-pane -t bob -p   # leer respuesta

# Con workspace del proyecto (IBM i):
bob -w /Users/atodamadresiempre/<proyecto> -p "..."

# Headless estructurado (JSON):
bob run "tarea" --format json --max-cost 1.0

# Reanudar tarea previa:
bob -r <task-id>
```

### Modos útiles

- `--max-cost <n>` — límite de costo por tarea (control corporativo)
- `--max-turns <n>` — límite de turnos
- `--mode agent|plan` — modo ejecución o planificación
- `bob mcp list` — servidores MCP configurados (hoy: ninguno)

### Costos observados

- Test "BOB-OK" (1 turno, sin tools): $0.017, 1.6s — 2026-09-07