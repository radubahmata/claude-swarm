# Kimi API-key smoke

Run one read-only shell command that verifies all of these conditions:

- `SWARM_DRIVER` is `kimi-cli`.
- `SWARM_AUTH_MODE` is `key`.
- `SWARM_EFFORT` and `KIMI_MODEL_THINKING_EFFORT` are `high`.
- `KIMI_MODEL_API_KEY` is non-empty. Do not print its value.
- `KIMI_MODEL_NAME` is `kimi-for-coding`.
- `git rev-parse --is-inside-work-tree` prints `true`.

Print `KIMI_APIKEY_HIGH_OK` only after every check passes. If a check fails,
report the failed condition and do not print the success marker.

Do not create, edit, commit, or push files. Exit after reporting the result.
