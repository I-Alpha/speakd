# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.2.0] - 2026-06-22

### Changed
- Renamed project from `speakd` to `voxcaster`. PyPI: `pip install voxcaster`.

### Added
- Markdown preprocessing: `strip_markdown()` removes code fences, URLs, headers, and formatting before synthesis. On by default (`markdown_preprocess = true`).
- TTS_SUMMARY extraction: embed `<!-- TTS_SUMMARY concise version TTS_SUMMARY -->` in text and only the summary is spoken — ideal for Claude Code hook integration.
- Per-request voice override: `speak("text", voice="bf_emma")` and `speak --voice bf_emma` override the daemon's configured voice for a single line.
- `ping()` / `speak --ping`: health-check the daemon — returns `ready`, `starting`, or `down` without triggering synthesis.
- `always_interrupt` config flag: every incoming request preempts in-flight playback (useful for Claude Code hooks where only the latest narration matters).

## [0.1.0] - 2026-06-11

### Added
- Initial public release.
- `voxcaster` daemon: Kokoro TTS behind a Unix domain socket with a FIFO
  narration queue, flock singleton, and stale-socket cleanup.
- Dynamic GPU offload: the model rides the GPU during narration bursts and
  releases its VRAM after a configurable idle keepalive.
- Wire protocol: fire-and-forget speak, interrupt (drain queue + cut
  playback), and live volume control.
- `speak` client CLI and a stdlib-only Python API (`speak`, `set_volume`,
  `ensure_daemon`) with daemon auto-spawn and graceful espeak fallback.
- TOML configuration with `VOXCASTER_*` environment overrides.
