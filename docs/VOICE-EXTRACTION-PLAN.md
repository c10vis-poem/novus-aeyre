# Extracting the voice pipeline into this APK

## What moves

From `aesop-xi/voice-engine/` — the real-time engine
(`scripts/live_voice_loop.py` + its launcher) and the working audio bridge
configuration.

## What it becomes

Native Android. The current implementation assumes Termux + a Debian proot with
PulseAudio. The APK has neither — it has AAudio/Oboe and Android's own audio
stack. **This is a rewrite, not a port.** The parts worth carrying over are the
*decisions*, not the code:

| Carry over | Drop |
|---|---|
| The 10s STT chunking rule | PulseAudio config |
| VAD gating thresholds that worked | proot paths |
| Model choices (Moonshine, Kokoro, Silero) | Python launcher shell scripts |
| The round-trip verification method (`--demo`) | ALSA-over-Pulse routing |

## Order

1. Get audio capture + playback working natively (AAudio). Verify with a
   round-trip that matches text, same as `--demo` did.
2. Add VAD gating.
3. Add STT **with the 10s chunking rule built in from the start** — not bolted on.
4. Add TTS.
5. Only then wire the LLM callback.
6. Verify the **live mic loop** — still unverified today, `--demo` is not proof.

## Do not

Start on vision until voice round-trips natively. Two unfinished media paths in
one daemon is how both end up broken.
