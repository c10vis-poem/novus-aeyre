# IPC — what Æyre speaks

Specs authored in `aesop-xi`. This records what this daemon implements.

| Channel | Transport | Used for |
|---|---|---|
| Daemon <-> daemon | `AF_UNIX` + `SCM_RIGHTS` | FD passing to/from Æsc |
| Daemon <-> UI | WebSocket | Horizons-Ui voice/vision surface |
| Audio + frame buffers | `ASharedMemory` | Zero-copy — copying frames costs more than inference |

## Process isolation

Own Android process. A vision crash must not take audio down with it, and
neither should touch Æsc.

## Undecided

- Backpressure when STT is slower than capture (drop frames? buffer? refuse?)
- Whether vision and audio share one foreground service or take two

Track in `novae-xorpus/unresolved.md`.
