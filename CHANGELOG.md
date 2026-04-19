# Changelog

All notable changes to this workspace-reshape effort are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-04-19

### Added
- 25 new cluster plugins built from scratch or from absorbed templates, plus extensions to 5 existing plugins, bringing the marketplace to 29 plugins.
- `marketplace.json` v2.0.0 release in the `Claude-Code-Plugins` repo.
- `find-template` skill in the workspace-foundational plugin and per-variant CLAUDE.md lenses across multi-variant plugins.

### Changed
- Consolidated 28 plugin clusters spanning the pilot, three reshape waves, and the three already-stable plugins (security-checkup, claude-user-memory, ai-attribution).
- Refreshed local Claude install: 26 stale plugins uninstalled and 25 new cluster plugins installed (0 install failures).
- Updated `Claude-Code-Projects-Index` (commit `6198d00`) — `data/marketplace.json` synced, `plugin_group_map` refreshed, category pages regenerated with cross-cluster "see also" pointers, and broken links cleaned up. Docs site auto-redeployed from Vercel.
- Renamed `Claude-Transcription-plugin` to `Claude-Transcription-Plugin` and added it as the 29th plugin.

### Removed
- 127 absorbed template repos and retired plugin repos deleted from GitHub (126 first pass + 1 retry after the rename); 3 were already gone. Per-repo log in `deletion-results.md`.
