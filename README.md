# pi-tps
A [pi coding agent](https://github.com/earendil-works/pi) extension that displays **live tokens-per-second (TPS)** in the footer during AI response generation.

## Features

- **Real-time streaming TPS** — updates continuously as tokens are generated
- **Animated spinner** — rotating `⠋⠙⠹⠸⠼⠴⠦⠧⠇⠏` frames during generation
- **Color-coded performance** — green (≥100 t/s), yellow (30–100 t/s), red (<30 t/s)
- **Compact display** — token counts in K notation (e.g. `1.2k`), shortened to `t/s`
- **Completion summary** — checkmark with final stats: `tps · tokens in X.Xs`

## Display

**Streaming:**
```
⠇ 127.3 t/s ↓ 1.2k tokens
```

**Completed:**
```
✓ 127.3 t/s · 1.2k tokens in 9.4s
```

## Installation

### Via pi install (recommended)

```bash
pi install npm:pi-tps-was-taken
```

This installs the package and registers it in pi's settings automatically. Then reload pi with `/reload`.


## How It Works

The extension subscribes to four pi events:

| Event | Purpose |
|-------|---------|
| `agent_start` | Clears state on new prompt |
| `message_start` | Records start time, begins spinner animation |
| `message_update` | Calculates and displays live TPS from incremental `usage.output` |
| `message_end` | Shows final average TPS, stops spinner |

Uses `ctx.ui.setStatus("tps", ...)` to append TPS to the existing footer (pwd, context %, model) instead of replacing it.

## Requirements

- pi coding agent with TUI support
- Local llama.cpp model (streaming usage updates must be incremental, not deferred to stream end)

## License

MIT
