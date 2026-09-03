# Kimi automatic-auth smoke

Run one read-only shell command that verifies all of these conditions:

- `SWARM_DRIVER` is `kimi-cli`.
- `SWARM_AUTH_MODE` is `auto`.
- `SWARM_EFFORT` and `KIMI_MODEL_THINKING_EFFORT` are `max`.
- `KIMI_MODEL_API_KEY` is non-empty. Do not print its value.
- `$HOME/.kimi-code` exists and is writable.
- `KIMI_MODEL_NAME` is `kimi-for-coding`.
- `git rev-parse --is-inside-work-tree` prints `true`.

Print `KIMI_AUTO_MAX_OK` only after every check passes. If a check fails,
report the failed condition and do not print the success marker.

Do not create, edit, commit, or push files. Exit after reporting the result.
