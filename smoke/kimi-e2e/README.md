# Kimi and Claude E2E smoke

This swarm runs four read-only, one-session agents:

| Agent | Driver | Auth | Effort | Context |
|---|---|---|---|---|
| 1 | Kimi | OAuth | low | none |
| 2 | Kimi | API key | high | slim |
| 3 | Kimi | automatic | max | full |
| 4 | Claude | automatic | low | slim |

The automatic Kimi case expects both credentials so the dashboard should
label it `auto`. The Claude case accepts an API key, an OAuth token, or both.
No prompt writes to the repository, and `max_idle: 1` stops each container
after its first session.

## Credentials

Log Kimi in on the host so `$HOME/.kimi-code` exists, and export
`KIMI_API_KEY`. Configure at least one Claude credential by exporting
`ANTHROPIC_API_KEY` or `CLAUDE_CODE_OAUTH_TOKEN`.

The swarmfile references `$KIMI_API_KEY`; it does not contain a secret.

## Run

Run from the repository root:

```bash
SWARM_CONFIG=smoke/kimi-e2e/swarm.json \
  ./launch.sh start --dashboard
```

The launcher clones committed `HEAD`, not the uncommitted working tree.
Commit this directory on a disposable local branch before running it.

Without the dashboard, wait for all four agents and inspect their output:

```bash
SWARM_CONFIG=smoke/kimi-e2e/swarm.json ./launch.sh start
SWARM_CONFIG=smoke/kimi-e2e/swarm.json ./launch.sh wait

for agent in 1 2 3 4; do
  SWARM_CONFIG=smoke/kimi-e2e/swarm.json ./launch.sh logs "$agent"
done

./costs.sh
```

Each container should exit with code zero after logging one tool call and an
`idle limit reached` message. Check the exit codes and read the unfiltered
agent logs from the stopped containers:

```bash
source lib/project.sh
project=$(swarm_project_id "$(basename "$(git rev-parse --show-toplevel)")")

for agent in 1 2 3 4; do
  docker inspect -f '{{.State.ExitCode}}' "${project}-agent-${agent}"
  docker cp "${project}-agent-${agent}:/workspace/agent_logs" - \
    | tar -xOf - --wildcards '*agent_*.log' 2>/dev/null \
    | grep -E 'KIMI_.*_OK|CLAUDE_.*_OK'
done
```

The commands should print exit code `0` and these markers, in order:

```text
KIMI_OAUTH_LOW_OK
KIMI_APIKEY_HIGH_OK
KIMI_AUTO_MAX_OK
CLAUDE_AUTO_LOW_OK
```

The Kimi pricing values are intentionally synthetic: one dollar per million
input, output, and cached tokens. They only make the stats path easy to see.
Once usage extraction is implemented, each successful Kimi row in
`./costs.sh` should have nonzero tokens and cost. A zero result reproduces the
known pricing issue on the reviewed PR head.
