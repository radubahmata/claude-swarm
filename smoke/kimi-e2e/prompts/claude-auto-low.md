# Claude mixed-driver smoke

Run one read-only shell command that verifies all of these conditions:

- `SWARM_DRIVER` is `claude-code`.
- `SWARM_AUTH_MODE` is `key`, `oauth`, or `auto`.
- `SWARM_EFFORT` and `CLAUDE_CODE_EFFORT_LEVEL` are `low`.
- At least one of `ANTHROPIC_API_KEY` or `CLAUDE_CODE_OAUTH_TOKEN` is
  non-empty. Do not print either value.
- `git rev-parse --is-inside-work-tree` prints `true`.

Print `CLAUDE_AUTO_LOW_OK` only after every check passes. If a check fails,
report the failed condition and do not print the success marker.

Do not create, edit, commit, or push files. Exit after reporting the result.
