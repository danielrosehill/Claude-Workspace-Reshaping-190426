# Repos to delete

Requires explicit Daniel approval before `gh repo delete --yes` is executed.

Format: `<repo>` — absorbed into `<cluster-plugin>`

**Total: ~100 repos**

---

## Absorbed template repos (from New-Repo-From-Template manifest)

### → Claude-Filesystem-Organiser-Plugin
- Claude-FS-Organiser
- Claude-Gdrive-Organiser

### → Claude-Budgeting-Plugin
- Claude-Budget-Workspace-Template
- home-budget-helper-plugin *(local scratch duplicate; if it has a GH remote, delete that too)*

### → Claude-Shopping-Plugin
- Claude-Israel-Shopping-Recommender-Template
- Israel-Online-Shopping-Skill
- Claude-Shopping-Eval-Demo

### → Claude-AI-Engineering-Plugin
- Prompt-Factory-Template
- Prompt-Library-Template

### → Claude-Media-Library-Plugin
- Claude-Media-Library-Org-Template

### → Claude-Audio-Production-Plugin
- Claude-Audio-Workspace-Template
- Claude-Podcast-Production-Template
- Transcript-Cleanup-Template

### → Claude-Video-Production-Plugin
- Claude-Video-Reel-Org-Template
- Claude-ComfyUI-Workspace-Template
- Claude-AI-Cover-Art-Template

### → Claude-PR-Media-Work-Plugin
- Claude-Media-Monitor
- Claude-PR-And-Media-Monitoring-Workspace
- Claude-Comms-Strategist-Template

### → Claude-Smart-Home-Plugin
- Claude-Home-Assistant-Manager-Template
- Claude-Snapcast-Maintenance-Template
- Claude-Spotify-Network-Installer-Template
- Plex-Synology-Management-Template

### → Claude-Debugging-Plugin
- Claude-Debugging-Workspace
- Bug-Tracker-Template

### → Claude-Content-Writing-Plugin
- Claude-Writing-Space-Template
- Claude-Blog-Manager
- Opinion-Blog-Template
- Writing-Examples-Template
- Document-Templates

### → Claude-Career-Plugin
- Claude-Career-Planning-Template
- Claude-Job-Search-Strategist
- Claude-Salary-Research-Agent

### → Claude-Purchasing-Plugin
- Claude-Purchasing-Assistant
- Claude-Recommendation-Workspace-Template
- Claude-Rig-Planner
- Claude-Business-Continuity-Planner *(empty .claude/; scaffold not absorbed but repo is redundant)*

### → Claude-Research-Space-Plugin
- Claude-Deep-Research-Template
- Claude-Technical-Research-Template
- Claude-OSINT-Investigator
- Claude-Georeaction-Researcher
- Claude-Single-Company-Research
- Claude-Stack-Research-Workspace
- Company-Research
- Ecosystem-Mapper
- Ecosystem-Mapper-Template
- Geo-Ecosystem-Template
- Tech-Radar-Template
- Claude-Competitor-Research-Agent

### → Claude-Legal-Investigative-Plugin
- Claude-Code-Lawyer
- Claude-Evidence-Assistant
- Claude-Lawyer-ISR
- Claude-Legal-Aid-Clinic
- Claude-Redaction-And-Obfuscation
- Proofmode-Unpacker

### → Claude-Knowledge-Documentation-Plugin
- Claude-Resource-List-Builder
- Wiki-Template
- Process-Docs-Template
- SOP-Builder-Template
- Experiment-Report-Template

### → Claude-Personal-Planning-Plugin
- Claude-Diary-Planner-Template
- Claude-Health-Helper
- Claude-House-Search-Template
- Claude-Parenting-Planner
- Claude-Personal-Development-Workspace
- Claude-Preparedness-Planner
- Claude-Therapy-Tracker
- Claude-Spam-Warrior
- Im-Not-Okay

### → Claude-Ideation-Planning-Plugin
- Claude-Business-Idea-Evaluator
- Claude-Decision-Evaluation-Framework
- Claude-Github-Shortlister
- Claude-Red-Team-Buyer
- Conference-Simulation-Template
- Debate-Mapper-Template
- Feature-Ideas-Template
- Idea-Capture-Template
- Ideas-To-Reports-Template
- Ideation-Runner-Template
- Negotiation-Simulation-Template
- Project-Ideas-Template
- Project-Planning-Template
- WIP-Idea-Template
- Claude-Think-Tank

### → Claude-Technical-Docs-Plugin
- Environment-Docs-Template
- Tech-Stack-Template
- Developer-Notebook-Template

### → Claude-Sysadmin-Homelab-Plugin
- Claude-ADB-Workspace-Template
- Claude-Code-LAN-Manager
- Claude-Conda-Manager
- Claude-Docker-Manager
- Claude-Linux-Server-Manager
- Claude-Proxmox-Manager-Template
- Claude-Server-Manager-Template
- Claude-Server-Mgmt-Template-SBCs
- Claude-Code-Remote-Machine-Admin-Space
- Claude-Synology-Manager
- Claude-MVT-Workspace
- Hardware-Probe-Agent-Template

### → Claude-Dev-Tools-Plugin
- Agent-Network-Expander-Template
- Agent-Task-Repo-Pattern-With-MCP
- Claude-Agent-Workspace-Model
- Claude-Repo-Creator
- Claude-Browser-Automation-Template
- General-Agent-Workspace-Template
- Personal-AI-Agent-Development-Template
- Templated-Repo-Kickstarter

### → Claude-Workspace-Foundational-Plugin
- Claude-Report-Parsing-Space-Template
- Claude-Research-Space-Public-Template
- Claude-Research-Workspace-General-Template
- Claude-Web-Analytics-Space
- Context-Maintenance-Template
- Inventory-Analyst-Template
- Report-Parsing-Space-Template *(may be same as above)*

---

## Retired plugin repos (absorbed into cluster plugins; marketplace entry already removed)

### → Claude-Technical-Docs-Plugin
- documentation-plugin (was `tech-docs`)
- Claude-Document-This (was `fix-documentation`)

### → Claude-Sysadmin-Homelab-Plugin
- bash-alias-manager-plugin
- bug-catcher-plugin *(partial — debugging primitives also went to Claude-Debugging-Plugin)*

### → Claude-Dev-Tools-Plugin
- Claude-Repo-Retrofitter
- Make-Agent-Friendly
- Claude-QA-Team *(was both plugin and template companion)*
- Claude-Templatizer
- Claude-Janitor
- Claude-Session-Transfer

### → Claude-Workspace-Foundational-Plugin
- workspace-setup-plugin
- New-Repo-From-Template-Plugin

### → Claude-Meta-Tools-Plugin
- new-turn-hook-plugin
- claudemd-chunker-plugin
- mcp-command-generator-plugin
- Claude-Code-What-Thing
- Claude-Code-Feedback-Plugin

---

## NOT in this list (confirmed stays)

- Claude-Security-Checkup-Plugin (renamed from security-checkup-plugin)
- Claude-User-Memory-Plugin (renamed from Claude-User-Memory-plugin)
- Claude-AI-Attribution-Plugin (renamed from ai-attribution-plugin)
- Claude-HP5200-Skill-plugin (standalone, single-purpose hardware plugin)
- Claude-Code-Plugins (the marketplace itself)
- Claude-Code-Projects-Index (the index repo)
- Claude-Workspace-Reshaping-190426 (this planning repo)
- All 24 new cluster plugins (the migration targets)

---

## Execution plan once approved

1. For each repo in this list: `gh repo delete danielrosehill/<repo> --yes`
2. Remove the corresponding local clone under `~/repos/github/my-repos/<repo>/`
3. Log the deletion in `conversion-log.md`

Estimated total: **~100 deletes**. Irreversible (GitHub's repo-deletion has no undo once confirmed).
