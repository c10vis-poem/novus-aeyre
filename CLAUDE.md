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

## Operator Rule 1 — no action without an explicit prompt

A skipped or unanswered question is NOT consent. No action — reading,
searching, or anything else — without an explicit prompt or permitted
request. State-changing or not, it doesn't matter.

## Operator Rule 2 — read this file and RESUME.md first

Before doing anything else in this repo, read this CLAUDE.md and RESUME.md.
Standing convention across the operator's repos for months — step one,
every session, no exceptions.

## Git workflow — push directly to main

Push directly to `main`. No feature branches, no PRs — operator directive
2026-09-15, superseding the previous branch/PR-required convention (that
workflow left work stranded on unmerged branches across sessions and
devices instead of ever reaching a shared, cloneable state). Before every
push, scan the diff for secrets/keys and refuse to push if any are found.
