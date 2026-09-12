# agent.md — novus-aeyre

Procedural directives for an agent working in this repo. See `CLAUDE.md` for
stack position, `README.md` for the full component/status breakdown.

## What to do here

- This is the media daemon (voice + vision). Voice is a working scaffold
  extracted from `aesop-xi/voice-engine/`; vision has no code yet.
- Before touching audio: read the `aesop-voice-pipeline` skill and the two
  documented gotchas in `README.md` (Moonshine's ~10s silent-failure ceiling,
  the two-part PulseAudio/proot bridge fix). Both have already cost real time
  once — don't rediscover them.
- Contracts for IPC (Æsc, Horizons-Ui) are authored in `aesop-xi`, not here —
  this repo implements them, doesn't define them.
- Weights and engine specs live in `novaexopia`; this repo runs them, doesn't
  own them.

## What NOT to do

- Don't rebuild the voice pipeline from scratch — it already works via
  `--demo` (TTS → speaker → STT round-trip verified). Extract/port it, don't
  redesign it.
- Don't trust the live mic loop as verified — only `--demo` has run. Verify
  before building further on it.
- Don't design the vision path without checking `aesop-xi` and `novaexopia`
  docs first for anything already decided about it.

## Source material status

Raw source documents for this repo (the Lex-Novi `Æsc&Æyre` subfolder, 14
items — daemon configs, ADB loopbacks, speech pipelines) have already been
audited and fully synthesized into `novaexopia`'s `README_NOVAEXOPIA.md` and
`01_SOVEREIGN_NODE_AND_APK_TOPOLOGY.md`, per
`AUDIT_LEX_NOVI_AESOP_XI.md` (Drive, `__LEX-NOVI-Review-OUTPUT`). The raw
subfolder itself is not shared with this Claude Code session — only the audit
summary is. This repo's own `README.md`/`CLAUDE.md` already reflect a more
current, more detailed state than that audit summary (the two documented
gotchas aren't in the audit doc at all), so treat this repo's own docs as the
better source, not the Drive audit.
