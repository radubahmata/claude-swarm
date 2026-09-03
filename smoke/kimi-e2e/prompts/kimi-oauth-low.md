# Kimi OAuth smoke

Run one read-only shell command that verifies all of these conditions:

- `SWARM_DRIVER` is `kimi-cli`.
- `SWARM_AUTH_MODE` is `oauth`.
- `SWARM_EFFORT` and `KIMI_MODEL_THINKING_EFFORT` are `low`.
- `$HOME/.kimi-code` exists and is writable.
- `git rev-parse --is-inside-work-tree` prints `true`.

Print `KIMI_OAUTH_LOW_OK` only after every check passes. If a check fails,
report the failed condition and do not print the success marker.

Do not create, edit, commit, or push files. Exit after reporting the result.
