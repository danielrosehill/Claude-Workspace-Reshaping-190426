# Domain Cluster Map (revised 2026-04-19)

27 target plugins, refined per Daniel's 2026-04-19 review. Narrower clusters preferred over broad ones to minimize context load when primitives are invoked.

The standalone `hp5200-printer` plugin stays as-is outside this map — no hardware cluster is needed.

---

## 1. research-space

**Rationale:** Open-ended investigation primitives — `/source-log`, `/summarize-sources`, `/deep-dive`. Narrowed: excludes career/salary (→ `career`) and rig selection (→ `purchasing`).

**Templates:** Claude Deep Research Template, Claude Technical Research Template, Claude OSINT Investigator, Claude Georeaction Researcher, Claude Single Company Research, Claude Stack Research Workspace, Company Research, Ecosystem Mapper, Ecosystem Mapper Template, Geo Ecosystem Template, Tech Radar Template, Claude Competitor Research Agent.

**Sub-variants:** `deep-research`, `technical`, `osint`, `georeaction`, `stack`, `ecosystem`, `competitor`.

**Verdict:** CREATE_PLUGIN.

---

## 2. career

**Rationale:** Career-specific primitives — `/log-role`, `/compare-offer`, `/track-application`, `/salary-benchmark`. Split out of research+personal-planning so salary/job context doesn't require loading general research machinery.

**Templates:** Claude Career Planning Template, Claude Job Search Strategist, Claude Salary Research Agent.

**Sub-variants:** `career-planning`, `job-search`, `salary-research`.

**Verdict:** CREATE_PLUGIN.

---

## 3. purchasing

**Rationale:** Purchasing decision primitives — `/compare-products`, `/spec-requirements`, `/source-quotes`. Generic purchasing workflows, independent of region-specific shopping or household budgeting.

**Templates:** Claude Purchasing Assistant, Claude Recommendation Workspace Template, Claude Rig Planner, Claude Business Continuity Planner (procurement-adjacent).

**Sub-variants:** `general-purchasing`, `tech-procurement` (rig planner), `vendor-recommendations`.

**Verdict:** CREATE_PLUGIN.

---

## 4. shopping

**Rationale:** Region-specific consumer shopping primitives — `/find-local-product`, `/compare-local-vendors`, `/check-availability`. Distinct from purchasing because it's tuned to local retail/market quirks.

**Templates:** Claude Israel Shopping Recommender Template + the second region-specific shopping template (flag during conversion).

**Sub-variants:** `israel`, `<other-region>` — extend as more region-specific scaffolds arrive.

**Verdict:** CREATE_PLUGIN.

---

## 5. budgeting

**Rationale:** Household/personal budget tracking primitives — `/log-transaction`, `/forecast-budget`, `/categorize`. Kept separate from purchasing per 2026-04-19 decision.

**Templates:** Claude Budget Workspace Template.

**Sub-variants:** none initially (single-purpose).

**Verdict:** CREATE_PLUGIN.

---

## 6. desktop-manager

**Rationale:** Standalone cluster Daniel regards as essential. Primitives for managing his own Linux desktop — `/check-system`, `/install-package`, `/apply-config`, `/troubleshoot-hardware`. Explicitly separate from `sysadmin-homelab` (which is remote/server-focused).

**Templates:** *(none in manifest — scaffold to be built from scratch around Daniel's workstation context)*.

**Sub-variants:** none initially.

**Verdict:** CREATE_PLUGIN (scaffold needs authoring).

---

## 7. sysadmin-homelab

**Rationale:** Remote/server admin primitives — `/diagnose`, `/update-config`, `/check-status`, `/manage-services`. Desktop is explicitly out (→ `desktop-manager`).

**Existing plugins absorbed:** `bash-alias-manager`, `bug-catcher`.

**Templates:** Claude ADB Workspace Template, Claude Code LAN Manager, Claude Conda Manager, Claude Docker Manager, Claude Linux Server Manager, Claude Proxmox Manager Template, Claude Server Manager Template, Claude Server Mgmt Template SBCs, Claude Code Remote Machine Admin Space, Claude Synology Manager, Claude MVT Workspace, Hardware Probe Agent Template, Plex Synology Management Template.

**Sub-variants:** `linux-servers`, `docker`, `conda`, `proxmox`, `nas-synology`, `android-debug`, `single-board-computers`, `remote-admin`, `lan`.

**Verdict:** EXTEND_EXISTING_PLUGIN.

---

## 8. filesystem-organiser

**Rationale:** Top-level per Daniel's 2026-04-19 directive. Filesystem organization primitives — `/scan-tree`, `/dedupe`, `/normalize-naming`, `/archive-old`. Niches handle different surfaces (local, remote-cloud).

**Templates:** Claude FS Organiser, Claude Gdrive Organiser.

**Sub-variants:** `local-desktop`, `gdrive`, `dropbox` (future), `nas` (future).

**Verdict:** CREATE_PLUGIN.

---

## 9. technical-docs

**Rationale:** Code/API documentation primitives — `/extract-api`, `/generate-reference`, `/version-doc`. Distinct from general content writing.

**Existing plugins absorbed:** `tech-docs`, `fix-documentation`.

**Templates:** Environment Docs Template, Tech Stack Template, Developer Notebook Template.

**Sub-variants:** `api-reference`, `code-docs`, `environment-docs`, `dev-notebook`.

**Verdict:** EXTEND_EXISTING_PLUGIN.

---

## 10. content-writing

**Rationale:** Prose/publishing primitives — `/draft`, `/improve-tone`, `/export`. Narrowed: excludes prompts (→ `ai-engineering`), technical reference (→ `technical-docs`), PR/media monitoring (→ `pr-media-work`).

**Templates:** Claude Writing Space Template, Claude Blog Manager, Opinion Blog Template, Writing Examples Template, Writing Space Template, Document Templates.

**Sub-variants:** `general-writing`, `blog`, `opinion-piece`, `document-templates`.

**Verdict:** CREATE_PLUGIN.

---

## 11. ai-engineering

**Rationale:** Prompt/LLM engineering primitives — `/craft-prompt`, `/eval-prompt`, `/catalog-prompt`, `/version-prompt`. Split out of docs-writing per 2026-04-19 — these are engineering artifacts, not prose.

**Templates:** Prompt Factory Template, Prompt Library Template.

**Sub-variants:** `prompt-library` (catalog), `prompt-factory` (generation + eval).

**Verdict:** CREATE_PLUGIN.

---

## 12. pr-media-work

**Rationale:** Work-focused PR/media-monitoring primitives — `/scan-coverage`, `/summarize-press`, `/draft-response`. Distinct from personal content writing; oriented toward professional communications.

**Templates:** Claude Media Monitor, Claude PR And Media Monitoring Workspace, Claude Comms Strategist Template.

**Sub-variants:** `media-monitoring`, `pr-response`, `comms-strategy`.

**Verdict:** CREATE_PLUGIN.

---

## 13. audio-production

**Rationale:** Audio-specific primitives — `/process-audio`, `/normalize-metadata`, `/vad`, `/transcribe`. The VAD pain point lives here; splitting audio from video makes that skill globally available without pulling in video tooling.

**Templates:** Claude Audio Workspace Template, Claude Podcast Production Template, Transcript Cleanup Template.

**Sub-variants:** `audio-engineering`, `podcast`, `transcript`.

**Verdict:** CREATE_PLUGIN.

---

## 14. video-production

**Rationale:** Video + generative media primitives — `/edit-clip`, `/generate-art`, `/render`. Distinct from audio.

**Templates:** Claude Video Reel Org Template, Claude ComfyUI Workspace Template, Claude AI Cover Art Template.

**Sub-variants:** `video-editing`, `generative-media`, `cover-art`.

**Verdict:** CREATE_PLUGIN.

---

## 15. media-library

**Rationale:** Asset library primitives — `/catalog`, `/tag`, `/search-library`. Kept separate from production to avoid loading editing tooling when only cataloging is needed.

**Templates:** Claude Media Library Org Template.

**Sub-variants:** none initially.

**Verdict:** CREATE_PLUGIN.

---

## 16. smart-home

**Rationale:** Home-automation primitives — `/discover-devices`, `/set-automation`, `/diagnose-network`, `/sync-config`.

**Templates:** Claude Home Assistant Manager Template, Claude Snapcast Maintenance Template, Claude Spotify Network Installer Template, Plex Synology Management Template.

**Sub-variants:** `home-assistant`, `multi-room-audio`, `media-server`.

**Verdict:** CREATE_PLUGIN.

---

## 17. personal-planning

**Rationale:** Non-career personal life primitives — `/log-entry`, `/review-progress`, `/set-goal`. Career moved to its own cluster.

**Templates:** Claude Diary Planner Template, Claude Health Helper, Claude House Search Template, Claude Parenting Planner, Claude Personal Development Workspace, Claude Preparedness Planner, Claude Therapy Tracker, Claude Spam Warrior, Im Not Okay.

**Sub-variants:** `diary`, `health-wellness`, `family-planning`, `house-search`, `preparedness`, `therapy-tracking`, `personal-dev`.

**Verdict:** CREATE_PLUGIN.

---

## 18. legal-investigative

**Rationale:** Legal research + investigation primitives — `/log-evidence`, `/analyze-document`, `/redact-content`, `/generate-brief`.

**Templates:** Claude Code Lawyer, Claude Evidence Assistant, Claude Lawyer ISR, Claude Legal Aid Clinic, Claude Redaction And Obfuscation, Proofmode Unpacker.

**Sub-variants:** `legal-research`, `evidence-management`, `osint-investigation`, `document-analysis`.

**Verdict:** CREATE_PLUGIN.

---

## 19. ideation-planning

**Rationale:** Brainstorming + idea evaluation primitives — `/brainstorm`, `/evaluate-idea`, `/rank-ideas`, `/synthesize-themes`. Per 2026-04-19: business idea evaluator lives here; two sub-variants cover single-idea eval vs multi-idea ranking.

**Templates:** Claude Business Idea Evaluator, Claude Decision Evaluation Framework, Claude Github Shortlister, Claude Red Team Buyer, Conference Simulation Template, Debate Mapper Template, Feature Ideas Template, Idea Capture Template, Ideas To Reports Template, Ideation Runner Template, Negotiation Simulation Template, Project Ideas Template, Project Planning Template, WIP Idea Template, Claude Think Tank.

**Sub-variants:** `ideation-session`, `single-idea-eval` (Business Idea Evaluator pattern), `multi-idea-ranking` (Github Shortlister / Red Team Buyer pattern), `feature-ideas`, `decision-framework`, `simulation`, `idea-capture`.

**Verdict:** CREATE_PLUGIN.

---

## 20. debugging

**Rationale:** Bug/log/diagnosis primitives — `/capture-logs`, `/isolate-issue`, `/diagnose-error`, `/suggest-fix`. Kept separate from sysadmin (runtime admin) and dev-tools (repo scaffolding).

**Templates:** Claude Debugging Workspace, Bug Tracker Template.

**Sub-variants:** `code-debugging`, `system-debugging`, `issue-tracking`.

**Verdict:** CREATE_PLUGIN.

---

## 21. dev-tools

**Rationale:** Repo/agent development primitives — `/scaffold-repo`, `/audit-code`, `/retrofit-claude`, `/review`.

**Existing plugins absorbed:** `repo-retrofitter`, `make-agent-friendly`, `qa-team`, `claude-templatizer`, `claude-janitor`, `session-transfer`.

**Templates:** Agent Network Expander Template, Agent Task Repo Pattern With MCP, Claude Agent Workspace Model, Claude Repo Creator, Claude Browser Automation Template, General Agent Workspace Template, Personal AI Agent Development Template, Templated Repo Kickstarter.

**Orphan companion repos:** Claude-QA-Team (scaffold for qa-team plugin per 2026-04-19 dedup decision — absorb then delete).

**Sub-variants:** `repo-scaffold`, `retrofitting`, `code-review` (absorbs Claude-QA-Team), `templatization`, `session-continuity`, `browser-automation`, `agent-workspace`.

**Verdict:** EXTEND_EXISTING_PLUGIN.

---

## 22. workspace-foundational

**Rationale:** Generic workspace patterns that don't fit a domain — setup, context management, report parsing, inventory analysis, analytics workspace.

**Existing plugins absorbed:** `workspace-setup`, `new-repo-from-template` (meta template-discovery skill folds in here).

**Templates:** Claude Report Parsing Space Template, Claude Research Space Public Template, Claude Research Workspace General Template, Claude Web Analytics Space, Context Maintenance Template, Inventory Analyst Template, Report Parsing Space Template.

**Sub-variants:** `generic-workspace`, `context-management`, `report-parsing`, `inventory-analysis`, `web-analytics`, `template-discovery`.

**Verdict:** EXTEND_EXISTING_PLUGIN.

---

## 23. knowledge-documentation

**Rationale:** Structured knowledge base primitives — `/index-topic`, `/cross-link`, `/build-taxonomy`. Distinct from content-writing (prose) and ai-engineering (prompts).

**Templates:** Claude Resource List Builder, Wiki Template, Process Docs Template, SOP Builder Template, Experiment Report Template.

**Sub-variants:** `wiki`, `resource-library`, `process-documentation`, `experiment-report`.

**Verdict:** CREATE_PLUGIN.

---

## 24. meta-tools

**Rationale:** Tools about Claude Code itself — context management, MCP, feedback, primitive discovery.

**Existing plugins absorbed:** `new-turn-hook`, `claudemd-chunker`, `mcp-command-generator`, `what-thing`, `claude-code-feedback`.

**Templates:** none.

**Sub-variants:** `context-management`, `mcp-integration`, `feedback-filing`, `primitive-selection`.

**Verdict:** EXTEND_EXISTING_PLUGIN.

---

## 25. security-compliance

**Rationale:** Security hardening + audit primitives — `/scan-vulnerability`, `/audit-config`, `/harden-system`.

**Existing plugins absorbed:** `security-checkup`.

**Templates:** none.

**Sub-variants:** `vulnerability-scanning`, `system-hardening`, `compliance-audit`.

**Verdict:** EXTEND_EXISTING_PLUGIN (stable).

---

## 26. user-memory-personalization

**Rationale:** Persistent context primitives.

**Existing plugins absorbed:** `claude-user-memory`.

**Verdict:** EXTEND_EXISTING_PLUGIN (stable).

---

## 27. ai-transparency-attribution

**Rationale:** AI/human contribution tracking primitives.

**Existing plugins absorbed:** `ai-attribution`.

**Verdict:** EXTEND_EXISTING_PLUGIN (stable).

---

## Out of cluster (standalone)

- **hp5200-printer** — existing plugin, single hardware target. Stays as standalone plugin.

---

## Templates staying as plain template repos

Per inventory, ~11 scaffold-only templates with no Claude behavior stay as standalone template repos (user invokes via `gh repo create --template`):

Templated-Repo-Kickstarter (note: if primitive-bearing version exists, move to dev-tools), Hardware-Probe-Agent-Template (note: if behavior exists, to sysadmin-homelab), Agent-Task-Repo-Pattern-With-MCP, Claude-Audio-Workspace-Template (verify — may have behavior), Claude-Business-Continuity-Planner (verify), Claude-Code-LAN-Manager (verify), Claude-Georeaction-Researcher (verify), Document Templates (verify), plus others flagged `TEMPLATE_SCAFFOLD_ONLY` in `inventory.md`.

These need a final pass during conversion — some may turn out to have behavior after all.

---

## Summary

- **27 cluster plugins** in the target marketplace + 1 standalone (`hp5200-printer`).
- **7 existing plugins** extended (tech-docs, fix-documentation, sysadmin-homelab absorbing bash-alias + bug-catcher, dev-tools absorbing 6, workspace-foundational absorbing 2, meta-tools absorbing 5, security-compliance, user-memory, ai-attribution).
- **20 new plugins** created from template consolidations.
- **111 templates** → ~100 absorbed, ~11 remain as plain template repos.
- **Biggest consolidation wins:**
  1. `ideation-planning` — 15 templates → 1 plugin
  2. `sysadmin-homelab` — 13 templates + 2 plugins → 1 plugin
  3. `research-space` — 12 templates → 1 plugin
  4. `dev-tools` — 8 templates + 6 plugins + 1 orphan → 1 plugin
  5. `personal-planning` — 9 templates → 1 plugin
  6. `legal-investigative` — 6 templates → 1 plugin
  7. `content-writing` — 6 templates → 1 plugin
  8. `meta-tools` — 5 plugins → 1 plugin
  9. `smart-home` — 4 templates → 1 plugin
  10. `knowledge-documentation` — 5 templates → 1 plugin

**Context shift:** narrower clusters (27 vs 18) keep each plugin's primitive surface tight, so invoking a career-research skill doesn't load general-research tooling.
