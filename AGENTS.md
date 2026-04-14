# AGENTS.md

This repository builds the `youtubearr` plugin for `m3u-editor`.

## Guardrails

- Keep `plugin.json` and `Plugin.php` as the runtime source of truth.
- Do not add top-level executable code outside the plugin class.
- Keep runtime files reviewable and minimal.
- Do not widen manifest permissions without updating the README and release notes.
- Package only runtime files for release artifacts.
- The plugin uses no custom database tables — channel tracking is via `Channel.info->plugin = 'youtubearr'`.

## Security

- GitHub CI is a quality signal, not a trust boundary.
- The host still performs reviewed install, ClamAV scanning, explicit trust, and integrity verification.
- `network_egress` and `filesystem_write` are high-risk permissions — both are required here (yt-dlp subprocess needs temp cookie files; stream detection makes outbound HTTPS calls to YouTube).
- Cookies are written to temp files during plugin runs and deleted before the action handler returns. Never persist cookies to permanent storage.

## yt-dlp subprocess

- All yt-dlp calls go through `runProcess()` which uses the Laravel Process facade with an explicit timeout.
- `findYtDlp()` checks common paths — do not hardcode `/usr/bin/yt-dlp`.
- The `--no-warnings` flag suppresses noisy stderr output; check `exitCode()` to detect failures.
