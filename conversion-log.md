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
| desktop-manager | ✅ | [`desktop-manager-plugin`](https://github.com/danielrosehill/desktop-manager-plugin) | *(scaffold authored from scratch)* | — | 8 commands, flat template, no variants. Validates clean. |
| budgeting | ✅ | [`budgeting-plugin`](https://github.com/danielrosehill/budgeting-plugin) | Claude-Budget-Workspace-Template | — | 10 commands + 6 agents. Also: `home-budget-helper-plugin` local dir is a scratch duplicate → deletion candidate. |
| shopping | ✅ | [`shopping-plugin`](https://github.com/danielrosehill/shopping-plugin) | Claude-Israel-Shopping-Recommender-Template, Israel-Online-Shopping-Skill, Claude-Shopping-Eval-Demo | — | 26 commands. All three sources were Israel-focused — shipped `generic` variant placeholder for future regions. Daniel to confirm if "other region" template exists somewhere. |
| ai-engineering | ✅ | [`ai-engineering-plugin`](https://github.com/danielrosehill/ai-engineering-plugin) | Prompt-Factory-Template, Prompt-Library-Template | — | 7 commands + 1 agent. System-Prompt-Factory (Streamlit app) + prompt-library-conversions (tracker) skipped — not primitive-bearing. |
| media-library | ✅ | [`media-library-plugin`](https://github.com/danielrosehill/media-library-plugin) | Claude-Media-Library-Org-Template | — | 10 commands + 1 agent. |

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
