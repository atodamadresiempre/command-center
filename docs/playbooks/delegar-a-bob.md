# Playbook: Delegar trabajo de IBM i a Bob

## Objetivo

Delegar tareas de desarrollo IBM i (RPG, CL, Db2 SQL, análisis de spooled
files, generación de código) al agente IBM Bob desde Hermes.

## Prerequisitos

- `bob` instalado (`~/.npm-global/bin/bob`)
- Autenticación SSO completada (keychain "IBM Bob Safe Storage")
- Licencia aceptada (`~/.bob/settings/settings.json` → licenseConsent: true)

## Regla crítica: TTY

Bob lee su API key del macOS Keychain, y el keychain SOLO es accesible con
TTY. Toda invocación desde Hermes DEBE usar PTY:

```bash
# Desde el tool terminal de Hermes (background=true, pty=true):
tmux new-session -d -s bobtask 'bob -p "Escribe un programa RPG FREE que lea el archivo CUSTOMERS y genere un reporte de saldos vencidos"'
sleep 30 && tmux capture-pane -t bobtask -p | tail -40
tmux kill-session -t bobtask
```

Alternativa sin TTY: exportar `BOB_API_KEY` (key scope Inference generada en
bob.ibm.com → API keys) y usar `bob run` directo. Pendiente de decisión.

## Pasos

1. **Definir la tarea** — prompt claro y acotado, en el idioma que Bob maneje
   mejor (inglés recomendado para código).
2. **Acotar el costo** — usar `--max-cost` en tareas grandes.
3. **Ejecutar con PTY** — patrón tmux de arriba.
4. **Verificar el resultado** — leer output; el resumen incluye costo,
   duración y Task ID.
5. **Persistir el Task ID** — anotarlo en la ficha del proyecto si la tarea
   puede necesitar continuación (`bob -r <task-id>`).

## Verificación

- Respuesta del asistente presente en el capture-pane.
- Costo dentro de lo presupuestado.
- Si la tarea tocó código: revisar diffs con `git status` en el workspace.

## Rollback

Bob NO escribe en repositorios directamente salvo que se le dé workspace.
Con `bob -p` (sin -w) solo consulta. Para deshacer trabajo con -w: git
checkout / restore en el workspace.

## Costos de referencia

- Turno simple sin tools: ~$0.017
- Presupuesto por tarea recomendado: --max-cost 1.0 (ajustar por experiencia)