# Kimi automatic-auth smoke

Use `smoke/kimi-e2e/results/kimi-auto-max.md` as the result file. If it
already exists, make no changes, print `KIMI_AUTO_MAX_ALREADY_RECORDED`, and
exit.

Otherwise, verify all of these conditions:

- `SWARM_DRIVER` is `kimi-cli`.
- `SWARM_AUTH_MODE` is `oauth` because OAuth is the only available Kimi
  credential.
- `SWARM_EFFORT` and `KIMI_MODEL_THINKING_EFFORT` are `max`.
- `$HOME/.kimi-code` exists and is writable.
- `git rev-parse --is-inside-work-tree` prints `true`.

Create the result file whether the checks pass or fail. Include the status,
agent ID, driver, auth mode, effort, context, Kimi version, base commit, UTC
time, and any failed conditions. Do not include credentials or their values.

Stage only the result file and commit it with this exact subject:

```text
Record Kimi automatic max smoke
```

Print `KIMI_AUTO_MAX_OK` after committing a passing result. Print
`KIMI_AUTO_MAX_FAIL` after committing a failing result. Do not modify any
other file.
