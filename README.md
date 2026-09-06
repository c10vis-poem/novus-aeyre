# Æyre

**Android media daemon — voice and vision.** Native APK. Repo: `novus-aeyre`.

Not a "voice repo." The media layer: audio in, audio out, and the camera/vision
path. Both halves live here because both are media-pipeline problems with the
same Android constraints (permissions, foreground service, hardware access,
zero-copy buffers).

One of three components that run **independently**: Horizons-Ui (UI), Æsc
(terminal), Æyre (media). None requires the others to start.

## Status

**Scaffold.** The voice pipeline exists and works elsewhere — it gets extracted
into this APK. Vision is not started.

## Voice — what already works (don't rebuild it)

Real-time engine confirmed working end-to-end via `--demo` (TTS -> speaker ->
STT round-trip, text matched). It currently lives in `aesop-xi/voice-engine/`
under Termux + Debian proot. **That arrangement is temporary** — it moves here
and gets wiped from `aesop-xi`.

### Two real bugs already found and fixed — do not rediscover these

**1. Moonshine STT has a ~10 second hard input ceiling.**
Past ~9.5-10s it fails **silently**. No exception, no warning, no truncation
notice — you just get wrong or empty output. Chunk audio before this limit.
This is the single most expensive gotcha in the voice stack.

**2. PulseAudio/proot audio bridge, two parts.**
- Missing ALSA-over-Pulse routing
- Client-side SHM must be disabled

Both were required. Fixing one and not the other looks like it still doesn't work.

Full detail lives in the `aesop-voice-pipeline` skill — read it before touching
audio.

### Not yet verified

**The live mic loop.** Only `--demo` has been run. Do not trust the mic path
blind — verify it before building on it.

## Components

| Layer | Model | Engine | Notes |
|---|---|---|---|
| VAD | Silero VAD | `silero-vad-engine` | Gates the pipeline. ONNX classifier. |
| STT | Moonshine Small (ONNX) | `moonshine-onnx-engine` | **~10s ceiling, silent failure.** |
| STT alt | NVIDIA Parakeet | whisper.cpp toolkit | At `~/tools/whisper-parakeet`. glibc — needs proot. |
| TTS | Kokoro 82M | `kokoro-tts-engine` | Confirmed working. |
| TTS alt | Piper | `piper-tts-engine` | PiperTTS-DE variant available. |
| Vision | LiteHRNet | — | Qualcomm-hosted. Not started. |

Weights and engine specs live in `novaexopia`. This repo runs them.

## Planned layout

```
app/
  daemon/       media daemon core (ForegroundService)
  audio/        capture, playback, PulseAudio/ALSA bridge
  vad/          Silero gating
  stt/          Moonshine + chunking that respects the 10s ceiling
  tts/          Kokoro / Piper
  vision/       camera capture + LiteHRNet  [not started]
protocol/       IPC contracts
docs/
```

## Android constraints that shape the design

- **ForegroundService is mandatory.** A media daemon that Android kills mid-session
  is not a daemon. Same blocker as Æsc — plain `Service` and `WorkManager` do not survive.
- **Permissions:** `RECORD_AUDIO`, `CAMERA`, and foreground-service types for both.
  Both are runtime-prompted and revocable — handle denial as a normal state, not a crash.
- **ASharedMemory** for audio and frame buffers. Copying every frame will cost more
  than the inference does.
- **Own process** (`android:process`) — a vision crash must not kill audio.

## IPC

Speaks to Æsc and Horizons-Ui over `AF_UNIX` + `SCM_RIGHTS` (FD passing),
WebSocket (UI), and `ASharedMemory` (buffers). Contracts are authored in
`aesop-xi`; this repo implements them.

## Open

- HTTP/WebSocket server for remote voice-engine invocation — spec'd in
  `aesop-xi/ARCHITECTURE.md` (Flask/FastAPI wrapping `VoiceEngine`), deliberately
  deferred, never built. See `novae-xorpus/unresolved.md` item 7.
- Vision path entirely — no design yet.

## Beginner-Proof Standard

Every module states the problem it solves, its data flow, and its failure mode.
The 10-second ceiling above is the template: state the trap, state why it's
invisible, state what to do instead.
