# Kimi and Claude E2E smoke

This swarm runs three agents that verify their environment and commit one
result file:

| Agent | Driver | Auth | Effort | Context |
|---|---|---|---|---|
| 1 | Kimi | OAuth | low | none |
| 2 | Kimi | automatic OAuth selection | max | full |
| 3 | Claude | OAuth | low | slim |

The two Kimi profiles use the same OAuth login but exercise explicit and
automatic auth selection, different effort levels, and different context
modes. Each first session commits a unique result. The next session sees the
result and makes no changes, so `max_idle: 1` stops the container.

## Credentials

Log Kimi in on the host so `$HOME/.kimi-code` exists. Generate a Claude OAuth
token with `claude setup-token`, then export `CLAUDE_CODE_OAUTH_TOKEN`.

## Run

Run from the repository root:

```bash
SWARM_CONFIG=smoke/kimi-e2e/swarm.json \
  ./launch.sh start --dashboard
```

The launcher clones committed `HEAD`, not the uncommitted working tree.
Commit this directory on a disposable local branch before running it.

Without the dashboard, wait for all three agents and inspect their output:

```bash
SWARM_CONFIG=smoke/kimi-e2e/swarm.json ./launch.sh start
SWARM_CONFIG=smoke/kimi-e2e/swarm.json ./launch.sh wait

for agent in 1 2 3; do
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

for agent in 1 2 3; do
  docker inspect -f '{{.State.ExitCode}}' "${project}-agent-${agent}"
  docker cp "${project}-agent-${agent}:/workspace/agent_logs" - \
    | tar -xOf - --wildcards '*agent_*.log' 2>/dev/null \
    | grep -E 'KIMI_.*_OK|CLAUDE_.*_OK'
done
```

The commands should print exit code `0` and these markers, in order:

```text
KIMI_OAUTH_LOW_OK
KIMI_AUTO_MAX_OK
CLAUDE_OAUTH_LOW_OK
```

After `launch.sh wait` harvests the commits, these files should exist and
report `PASS`:

```text
smoke/kimi-e2e/results/kimi-oauth-low.md
smoke/kimi-e2e/results/kimi-auto-max.md
smoke/kimi-e2e/results/claude-oauth-low.md
```

The Kimi pricing values are intentionally synthetic: one dollar per million
input, output, and cached tokens. They only make the stats path easy to see.
Once usage extraction is implemented, each successful Kimi row in
`./costs.sh` should have nonzero tokens and cost. A zero result reproduces the
known pricing issue on the reviewed PR head.
