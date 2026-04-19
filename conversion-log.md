# Conversion Log

Tracks per-cluster conversion progress. Updated as each cluster is built.

Legend: ⬜ pending · 🟡 in progress · ✅ complete · 🗑️ template repos ready for deletion

---

## Pilot

| Cluster | Status | Target plugin repo | Templates absorbed | Old plugins absorbed | Notes |
|---|---|---|---|---|---|
| filesystem-organiser | ✅ | [`Claude-Filesystem-Organiser-Plugin`](https://github.com/danielrosehill/Claude-Filesystem-Organiser-Plugin) | Claude-FS-Organiser, Claude-Gdrive-Organiser | — | Pilot complete. 21 commands + 1 agent promoted. Two variants (local, gdrive). `claude plugin validate` passed (warnings only: migrated commands lack YAML frontmatter — pre-existing, non-blocking). |

## Wave 1 — new plugins, single or small template count (clean creates)

| Cluster | Status | Target plugin repo | Templates absorbed | Old plugins absorbed | Notes |
|---|---|---|---|---|---|
| desktop-manager | ✅ | [`Claude-Desktop-Manager-Plugin`](https://github.com/danielrosehill/Claude-Desktop-Manager-Plugin) | *(scaffold authored from scratch)* | — | 8 commands, flat template, no variants. Validates clean. |
| budgeting | ✅ | [`Claude-Budgeting-Plugin`](https://github.com/danielrosehill/Claude-Budgeting-Plugin) | Claude-Budget-Workspace-Template | — | 10 commands + 6 agents. Also: `home-budget-helper-plugin` local dir is a scratch duplicate → deletion candidate. |
| shopping | ✅ | [`Claude-Shopping-Plugin`](https://github.com/danielrosehill/Claude-Shopping-Plugin) | Claude-Israel-Shopping-Recommender-Template, Israel-Online-Shopping-Skill, Claude-Shopping-Eval-Demo | — | 26 commands. All three sources were Israel-focused — shipped `generic` variant placeholder for future regions. Daniel to confirm if "other region" template exists somewhere. |
| ai-engineering | ✅ | [`Claude-AI-Engineering-Plugin`](https://github.com/danielrosehill/Claude-AI-Engineering-Plugin) | Prompt-Factory-Template, Prompt-Library-Template | — | 7 commands + 1 agent. System-Prompt-Factory (Streamlit app) + prompt-library-conversions (tracker) skipped — not primitive-bearing. |
| media-library | ✅ | [`Claude-Media-Library-Plugin`](https://github.com/danielrosehill/Claude-Media-Library-Plugin) | Claude-Media-Library-Org-Template | — | 10 commands + 1 agent. |

## Wave 2 — new plugins, larger template counts

| Cluster | Status | Target plugin repo | Templates absorbed | Old plugins absorbed | Notes |
|---|---|---|---|---|---|
| career | ✅ | [`Claude-Career-Plugin`](https://github.com/danielrosehill/Claude-Career-Plugin) | Claude-Career-Planning-Template, Claude-Job-Search-Strategist, Claude-Salary-Research-Agent | — | 14 commands + 1 agent, 3 variants. |
| purchasing | ✅ | [`Claude-Purchasing-Plugin`](https://github.com/danielrosehill/Claude-Purchasing-Plugin) | Claude-Purchasing-Assistant, Claude-Recommendation-Workspace-Template, Claude-Rig-Planner | — | 25 cmds + 7 agents, 3 variants. Business-Continuity-Planner had empty .claude/ — skipped. |
| research-space | ✅ | [`Claude-Research-Space-Plugin`](https://github.com/danielrosehill/Claude-Research-Space-Plugin) | 12 sources (Deep/Technical/OSINT/Georeaction/Single-Company/Stack/Ecosystem(×3)/Geo-Ecosystem/Tech-Radar/Competitor) | — | Aggressive dedup: 6 global commands + variant-specific CLAUDE.md lenses. 7 variants. |
| audio-production | ✅ | [`Claude-Audio-Production-Plugin`](https://github.com/danielrosehill/Claude-Audio-Production-Plugin) | Claude-Audio-Workspace-Template, Claude-Podcast-Production-Template, Transcript-Cleanup-Template | — | 19 commands + 1 agent + VAD skill. 3 variants. VAD globally reachable. |
| video-production | ✅ | [`Claude-Video-Production-Plugin`](https://github.com/danielrosehill/Claude-Video-Production-Plugin) | Claude-Video-Reel-Org-Template, Claude-ComfyUI-Workspace-Template, Claude-AI-Cover-Art-Template | — | 21 commands + 1 agent. 3 variants. |
| pr-media-work | ✅ | [`Claude-PR-Media-Work-Plugin`](https://github.com/danielrosehill/Claude-PR-Media-Work-Plugin) | Claude-Media-Monitor, Claude-PR-And-Media-Monitoring-Workspace, Claude-Comms-Strategist-Template | — | 28 cmds + 17 agents. Media-Monitor and PR-And-Media-Monitoring are byte-identical — deduped. |
| smart-home | ✅ | [`Claude-Smart-Home-Plugin`](https://github.com/danielrosehill/Claude-Smart-Home-Plugin) | Claude-Home-Assistant-Manager-Template, Claude-Snapcast-Maintenance-Template, Claude-Spotify-Network-Installer-Template, Plex-Synology-Management-Template | — | 47 cmds + 5 agents + 2 skills. Personal IPs (10.0.0.x) + hostnames (bedroompi, nurserypi) scrubbed. |
| legal-investigative | ✅ | [`Claude-Legal-Investigative-Plugin`](https://github.com/danielrosehill/Claude-Legal-Investigative-Plugin) | Claude-Code-Lawyer, Claude-Evidence-Assistant, Claude-Lawyer-ISR, Claude-Legal-Aid-Clinic, Claude-Redaction-And-Obfuscation, Proofmode-Unpacker | — | 4 core primitives, 4 variants. Israel kept as jurisdiction overlay (`--jurisdiction=israel`). |
| knowledge-documentation | ✅ | [`Claude-Knowledge-Documentation-Plugin`](https://github.com/danielrosehill/Claude-Knowledge-Documentation-Plugin) | Claude-Resource-List-Builder, Wiki-Template, Process-Docs-Template, SOP-Builder-Template, Experiment-Report-Template | — | 7 commands, 4 variants. SOP+Process-Docs merged. |
| content-writing | ✅ | [`Claude-Content-Writing-Plugin`](https://github.com/danielrosehill/Claude-Content-Writing-Plugin) | Claude-Writing-Space-Template, Claude-Blog-Manager, Opinion-Blog-Template, Writing-Examples-Template, Document-Templates | — | 4 variants. |
| personal-planning | ✅ | [`Claude-Personal-Planning-Plugin`](https://github.com/danielrosehill/Claude-Personal-Planning-Plugin) | Claude-Diary-Planner-Template, Claude-Health-Helper, Claude-House-Search-Template, Claude-Parenting-Planner, Claude-Personal-Development-Workspace, Claude-Preparedness-Planner, Claude-Therapy-Tracker, Claude-Spam-Warrior, Im-Not-Okay | — | 3 deduped variant-aware cmds + 1 agent, 8 variants (added `inbox-hygiene`). |
| ideation-planning | ✅ | [`Claude-Ideation-Planning-Plugin`](https://github.com/danielrosehill/Claude-Ideation-Planning-Plugin) | 15 sources (Business-Idea-Evaluator, Decision-Eval, GH-Shortlister, Red-Team-Buyer, Conference-Sim, Debate-Mapper, Feature/Idea/Project-Ideas, Ideas-To-Reports, Ideation-Runner, Negotiation-Sim, Project-Planning, WIP-Idea, Think-Tank) | — | 24 cmds + 4 agents, 7 variants. Think-Tank's off-scope primitives (podcast, press-release) excluded. |
| debugging | ✅ | [`Claude-Debugging-Plugin`](https://github.com/danielrosehill/Claude-Debugging-Plugin) | Claude-Debugging-Workspace, Bug-Tracker-Template | — | 10 commands + 2 agents. 3 variants. |

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
| Update Claude-Code-Projects-Index | ⬜ | After marketplace updates. Large update: remove ~100 deleted template repos, add 27 new plugin entries, update counts. Local clone dir is stale (`Claude-Code-Repos-Index` → rename to match). |
| Redeploy docs site from index | ⬜ | The index feeds a generated docs site. Find deploy mechanism (`.github/workflows/`, Cloudflare Pages, Vercel, etc.) and trigger. |
| Batch-delete absorbed template repos | ⬜ | **Checkpoint with Daniel before executing** — large, irreversible. |
| Archive New-Repo-From-Template-Plugin | ⬜ | Final step. |
