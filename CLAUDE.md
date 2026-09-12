# Æyre — voice and vision daemon

Canon name: **Æyre**. Repo name: `novus-aeyre`. See `novae-xorpus/NAMING-CANON.md`.

## What this is

The voice and vision daemon layer of the Æsop-Xi stack. Handles STT
(Moonshine/Whisper), TTS (Kokoro/termux-tts-speak), VAD (Silero), and
screen-vision context capture. Part of the 3-APK architecture alongside
Horizons-Ui and Æsc.

## Stack position

```
Æsop-Xi → NovÆxenti → NovÆxopia → Æsc (terminal)
                                 → Æyre (this repo)
```

Æyre runs independently. Horizons-Ui does NOT require Æyre to function.

## Conventions

- `screen-vision/` — real-time pixel canvas parsing and vision capture.
- `voice-ast-stack/` — native low-latency VAD, TTS, and STT pipelines.
- Protocol specs in `protocol/` — media daemon lifecycle, audio pipeline
  contract, VAD thresholds, TTS streaming.
- Reference docs in `docs/` — Moonshine/Kokoro setup, Silero VAD integration,
  screen-vision API, device profiles (Razr Ultra NPU vs Tab S9 vs base).

## Git workflow

PR required. No direct pushes to main. CI runs gitleaks + structure check.
