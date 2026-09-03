# Claude OAuth smoke

Use `smoke/kimi-e2e/results/claude-oauth-low.md` as the result file. If it
already exists, make no changes, print `CLAUDE_OAUTH_LOW_ALREADY_RECORDED`,
and exit.

Otherwise, verify all of these conditions:

- `SWARM_DRIVER` is `claude-code`.
- `SWARM_AUTH_MODE` is `oauth`.
- `SWARM_EFFORT` and `CLAUDE_CODE_EFFORT_LEVEL` are `low`.
- `CLAUDE_CODE_OAUTH_TOKEN` is non-empty. Do not print its value.
- `git rev-parse --is-inside-work-tree` prints `true`.

Create the result file whether the checks pass or fail. Include the status,
agent ID, driver, auth mode, effort, context, Claude version, base commit, UTC
time, and any failed conditions. Do not include credentials or their values.

Stage only the result file and commit it with this exact subject:

```text
Record Claude OAuth low smoke
```

Print `CLAUDE_OAUTH_LOW_OK` after committing a passing result. Print
`CLAUDE_OAUTH_LOW_FAIL` after committing a failing result. Do not modify any
other file.
