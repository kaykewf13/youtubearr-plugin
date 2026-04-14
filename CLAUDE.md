# CLAUDE.md

Work on `youtubearr` as a reviewable plugin artifact for `m3u-editor`.

## Expectations

- Keep the runtime surface centered on `plugin.json` and `Plugin.php`.
- Prefer small, explicit manifest changes over hidden behavior.
- Avoid top-level side effects in PHP files.
- Keep release artifacts reproducible with `bash scripts/package-plugin.sh`.
- Update the published checksum whenever the release zip changes.

## Key design decisions

- No URL refresh: permanent YouTube watch URL stored; proxy re-resolves via yt-dlp on each connection.
- No custom DB table: channel tracking is implicit via `Channel.info->plugin = 'youtubearr'`.
- User ID for scheduled runs: derived from the configured StreamProfile's `user_id` when `$context->user` is null.
- Cookies are written to temp files at the start of each action handler and cleaned up before the handler returns — not via `atexit`.
