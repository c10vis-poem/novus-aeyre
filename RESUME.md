# RESUME.md — novus-aeyre

**Last updated:** 2026-09-07
**Branch:** `restructure/drive-file-tree`

## Current state

Voice: working scaffold, confirmed end-to-end via `--demo` (TTS → speaker →
STT round-trip, text matched). Currently lives in `aesop-xi/voice-engine/`
under Termux + Debian proot — temporary, moves here and gets wiped from
`aesop-xi`. Live mic loop not yet verified — only `--demo` has run.

Vision: not started. No code, no design beyond the stub directories.

Two real bugs already found and fixed, documented in `README.md` — don't
rediscover them: Moonshine STT's ~10s silent-failure ceiling, and the
two-part PulseAudio/proot audio bridge fix (ALSA-over-Pulse routing +
client-side SHM disabled).

## What was done this session

Checked this repo's Drive source material (`Æsc&Æyre` subfolder of
`___Lex-Novi-Æxentis-Copiæ`, per `05_FEDERATED_FILE_TREE_TOPOLOGY_MASTER.md`'s
14-subfolder mapping table, entry `Reverse-Engineering`). Confirmed via
`AUDIT_LEX_NOVI_AESOP_XI.md` that this material was already fully synthesized
into `novaexopia/README_NOVAEXOPIA.md` and
`novaexopia/01_SOVEREIGN_NODE_AND_APK_TOPOLOGY.md` — out of scope for this
repo. The raw subfolder itself is not shared with this Claude Code session
(only the audit summary is), so nothing further was retrievable to place
here. Added the missing section-kit files (`agent.md`, this file,
`unresolved.md`) per the corrected per-repo layout in
`novae-xorpus/project_novae_xorpus_repo_layout_correction.md` (see project
memory, not committed to this repo).

## Next

- Extract the voice engine from `aesop-xi/voice-engine/` into `app/audio/`,
  `app/vad/`, `app/stt/`, `app/tts/` per the planned layout in `README.md`.
- Verify the live mic loop before building further on it.
- Start the vision path — no design exists yet; check `aesop-xi` and
  `novaexopia` first for anything already decided.
