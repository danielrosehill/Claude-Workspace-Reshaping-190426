# Conversion Log

Tracks per-cluster conversion progress. Updated as each cluster is built.

Legend: ⬜ pending · 🟡 in progress · ✅ complete · 🗑️ template repos ready for deletion

---

## Pilot

| Cluster | Status | Target plugin repo | Templates absorbed | Old plugins absorbed | Notes |
|---|---|---|---|---|---|
| filesystem-organiser | ✅ | [`filesystem-organiser-plugin`](https://github.com/danielrosehill/filesystem-organiser-plugin) | Claude-FS-Organiser, Claude-Gdrive-Organiser | — | Pilot complete. 21 commands + 1 agent promoted. Two variants (local, gdrive). `claude plugin validate` passed (warnings only: migrated commands lack YAML frontmatter — pre-existing, non-blocking). |

## Wave 1 — new plugins, single or small template count (clean creates)

| Cluster | Status | Target plugin repo | Templates absorbed | Old plugins absorbed | Notes |
|---|---|---|---|---|---|
| desktop-manager | ⬜ | new | *(scaffold authored from scratch)* | — | User-flagged essential. |
| budgeting | ⬜ | new | Claude Budget Workspace Template | — | |
| shopping | ⬜ | new | Claude Israel Shopping Recommender + other | — | |
| ai-engineering | ⬜ | new | Prompt Factory, Prompt Library | — | |
| media-library | ⬜ | new | Claude Media Library Org Template | — | |

## Wave 2 — new plugins, larger template counts

| Cluster | Status | Target plugin repo | Templates absorbed | Old plugins absorbed | Notes |
|---|---|---|---|---|---|
| career | ⬜ | new | Career Planning, Job Search, Salary Research | — | |
| purchasing | ⬜ | new | Purchasing Assistant, Recommendations, Rig Planner, Business Continuity | — | |
| research-space | ⬜ | new | 12 research templates | — | |
| audio-production | ⬜ | new | Audio Workspace, Podcast, Transcript Cleanup | — | |
| video-production | ⬜ | new | Video Reel Org, ComfyUI, Cover Art | — | |
| pr-media-work | ⬜ | new | Media Monitor, PR & Media, Comms Strategist | — | |
| smart-home | ⬜ | new | Home Assistant, Snapcast, Spotify, Plex-Synology | — | |
| legal-investigative | ⬜ | new | 6 legal templates | — | |
| knowledge-documentation | ⬜ | new | Resource List, Wiki, Process Docs, SOP, Experiment Report | — | |
| content-writing | ⬜ | new | Writing Space, Blog Manager, Opinion Blog, Writing Examples, Document Templates | — | |
| personal-planning | ⬜ | new | Diary, Health, Parenting, House Search, Personal Dev, Preparedness, Therapy, Spam, Im Not Okay | — | |
| ideation-planning | ⬜ | new | 15 ideation templates | — | |
| debugging | ⬜ | new | Debugging Workspace, Bug Tracker | — | |

## Wave 3 — extend existing plugins (absorb companions)

| Cluster | Status | Target plugin repo | Templates absorbed | Old plugins absorbed | Notes |
|---|---|---|---|---|---|
| technical-docs | ⬜ | existing `documentation-plugin` | Environment Docs, Tech Stack, Developer Notebook | `tech-docs`, `fix-documentation` | |
| sysadmin-homelab | ⬜ | new or existing bash-alias-manager repo | 13 sysadmin templates | `bash-alias-manager`, `bug-catcher` | |
| dev-tools | ⬜ | new or existing repo-retrofitter repo | 8 dev templates + Claude-QA-Team orphan | `repo-retrofitter`, `make-agent-friendly`, `qa-team`, `claude-templatizer`, `claude-janitor`, `session-transfer` | QA-Team dedup lives here. |
| workspace-foundational | ⬜ | existing `workspace-setup-plugin` | 7 foundational templates | `workspace-setup`, `new-repo-from-template` | |
| meta-tools | ⬜ | new | — | `new-turn-hook`, `claudemd-chunker`, `mcp-command-generator`, `what-thing`, `claude-code-feedback` | |
| security-compliance | ✅ | existing `security-checkup-plugin` | — | `security-checkup` | Already stable, no work. |
| user-memory-personalization | ✅ | existing `Claude-User-Memory-plugin` | — | `claude-user-memory` | Already stable. |
| ai-transparency-attribution | ✅ | existing `ai-attribution-plugin` | — | `ai-attribution` | Already stable. |

## Final stages

| Stage | Status | Notes |
|---|---|---|
| Update marketplace.json (remove new-repo-from-template, add/update all) | ⬜ | After all waves. |
| Refresh plugins locally | ⬜ | `claude plugin update` on this workstation. |
| Update Claude-Code-Repos-Index | ⬜ | After marketplace updates. |
| Batch-delete absorbed template repos | ⬜ | **Checkpoint with Daniel before executing** — large, irreversible. |
| Archive New-Repo-From-Template-Plugin | ⬜ | Final step. |
