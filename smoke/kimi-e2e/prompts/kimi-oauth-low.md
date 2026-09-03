# Kimi OAuth smoke

Use `smoke/kimi-e2e/results/kimi-oauth-low.md` as the result file. If it
already exists, make no changes, print `KIMI_OAUTH_LOW_ALREADY_RECORDED`, and
exit.

Otherwise, verify all of these conditions:

- `SWARM_DRIVER` is `kimi-cli`.
- `SWARM_AUTH_MODE` is `oauth`.
- `SWARM_EFFORT` and `KIMI_MODEL_THINKING_EFFORT` are `low`.
- `$HOME/.kimi-code` exists and is writable.
- `git rev-parse --is-inside-work-tree` prints `true`.

Create the result file whether the checks pass or fail. Include the status,
agent ID, driver, auth mode, effort, context, Kimi version, base commit, UTC
time, and any failed conditions. Do not include credentials or their values.

Stage only the result file and commit it with this exact subject:

```text
Record Kimi OAuth low smoke
```

Print `KIMI_OAUTH_LOW_OK` after committing a passing result. Print
`KIMI_OAUTH_LOW_FAIL` after committing a failing result. Do not modify any
other file.
