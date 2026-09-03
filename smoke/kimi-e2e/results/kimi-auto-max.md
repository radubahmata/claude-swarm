# Kimi automatic-auth max smoke

| Field | Value |
|---|---|
| Status | PASS |
| Agent ID | 2 |
| Driver | kimi-cli |
| Auth mode | oauth |
| Effort | max |
| Context | full |
| Kimi version | 0.40.1 |
| Base commit | ae8a054bc53513d8470380dc4bec8fbf3bd3a728 |
| UTC time | 2026-09-03T10:28:51Z |

## Checks

- `SWARM_DRIVER` is `kimi-cli`: pass
- `SWARM_AUTH_MODE` is `oauth`: pass
- `SWARM_EFFORT` is `max`: pass
- `KIMI_MODEL_THINKING_EFFORT` is `max`: pass
- `$HOME/.kimi-code` exists and is writable: pass
- `git rev-parse --is-inside-work-tree` prints `true`: pass

Failed conditions: none.
