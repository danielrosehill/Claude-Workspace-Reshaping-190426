# Claude Workspace Reshaping — Determinative Plan

This document is the prescriptive spec for refactoring Daniel's Claude Code plugin collection. Background, the original voice-typed task definition, and the review that led here are preserved in `docs/` and `inventory.md` / `clusters.md`.

---

## 1. The target pattern

Every workspace-oriented plugin in the marketplace follows this shape:

```
<plugin-repo>/
  .claude-plugin/
    plugin.json
  skills/
    new-workspace/
      SKILL.md              # provisioning skill
    <primitive-skills>/     # domain primitives, globally available
  commands/                 # optional domain commands
  agents/                   # optional domain subagents
  template/
    CLAUDE.md               # scaffold for the provisioned workspace
    README.md
    context/
    outputs/
  README.md
```

### Responsibilities

- **Primitives** (`skills/`, `commands/`, `agents/`) — installed globally from the marketplace, reachable from **any** cwd. They hold the reusable behavior (e.g. a VAD skill in the `media-production` plugin). They never live inside a provisioned workspace.
- **Scaffold** (`template/`) — a plain data scaffold shipped as plugin data. No `.claude/` tree. Copied into the user's target path when they invoke `new-workspace`.
- **Provisioning skill** (`skills/new-workspace/SKILL.md`) — takes a target path + workspace name + optional sub-variant. Copies the scaffold, personalizes CLAUDE.md using facts from `~/.claude/CLAUDE.md`, optionally creates a GitHub repo via `gh`.
- **User data** lives in the provisioned workspace. Plugin updates never overwrite it.

### Invariants

- Plugins published to the public marketplace contain **no** Daniel-specific paths, hostnames, or identifiers. Personalization happens at `new-workspace` time by reading user memory.
- The `template/` directory **must not** contain a `.claude/` subdirectory. If it did, those duplicates would shadow the plugin's globals and recreate the fusion problem we're solving (see `docs/usage-example.md` — the VAD pain point).
- Resolve the bundled scaffold from a skill via `${CLAUDE_SKILL_DIR}/../../template`. `${CLAUDE_PLUGIN_ROOT}` is only exported in hooks/MCP contexts, not in skill bash injection.
- Marketplace entries use `strict: true` (default). Each plugin's own `plugin.json` is authoritative.

### Provisioning skill contract

Every `new-workspace` skill:

1. Accepts `$ARGUMENTS` as `<workspace-name> [<target-path>] [--variant=<name>] [--local-only]`.
2. Default target path: `~/repos/github/my-repos/<workspace-name>`.
3. Default: create a **public** GitHub repo via `gh repo create` and push. `--local-only` skips the remote.
4. Reads `~/.claude/CLAUDE.md` to fill OS/locale/timezone/identity into the new workspace's CLAUDE.md.
5. Prompts only for facts it can't infer.
6. After provision, prints next-step hints (which plugin primitives apply, where to drop context files).

---

## 2. The refactor

### Decisions (locked)

- **Consolidate by primitives, not framing.** Templates collapse into clusters defined by the primitives they'd share. See `clusters.md` for the full cluster-to-plugin map.
- **Promote every behavior-bearing template** — either into a new cluster plugin, or by merging into an existing one per the cluster map.
- **Demote** any existing plugin that's really just a scaffold (inventory found zero; none to demote).
- **Keep as standalone template repo** only the ~11 pure scaffolds with no behavior (`TEMPLATE_SCAFFOLD_ONLY` in `inventory.md`).
- **Delete** absorbed template repos immediately after their cluster plugin is published and verified locally. No cooling period.
- **Public scope only** — this reshape covers the public `danielrosehill` marketplace. Private plugins are out of scope for this repo.
- **QA dedup** — the `qa-team` plugin absorbs the `Claude-QA-Team` scaffold. Old template repo deleted.

### Target plugin count

**27 cluster plugins + 1 standalone** (`hp5200-printer`) — see `clusters.md` for the definitive map. Replaces today's 21 scattered plugins + 111 template repos. A small residual set of plain template repos survives for pure scaffolds (per `inventory.md`).

---

## 3. Execution order

All conversions follow this sequence. Batch-run across clusters.

### Per-cluster conversion (repeat for each of the 18 clusters)

1. **Read the cluster entry in `clusters.md`** — pick the target plugin repo (new or existing), the templates to absorb, sub-variants.
2. **Scaffold or locate the plugin repo** at `~/repos/github/my-repos/<plugin-name>/`:
   - If extending an existing plugin, branch off its current state.
   - If creating new, scaffold: `.claude-plugin/plugin.json`, `README.md`, `skills/new-workspace/SKILL.md`, `template/` dir.
3. **Promote primitives** — copy skills/commands/agents from the source templates into the plugin's top-level `skills/`, `commands/`, `agents/`. Deduplicate near-identical primitives; one authoritative version per plugin.
4. **Assemble the scaffold** — merge the per-template scaffold content (CLAUDE.md text, context/ structure, README) into `template/`. Where sub-variants differ, keep variant-specific files under `template/variants/<variant>/`.
5. **Write the `new-workspace` skill** — per the contract in §1. Include `disable-model-invocation: true` and `allowed-tools: Bash(mkdir *), Bash(cp *), Bash(git init *), Bash(gh repo create *), Bash(git push *)`.
6. **Validate** locally: `claude plugin validate .`.
7. **Bump version** in `plugin.json` and commit. Push to GitHub.
8. **Record** absorbed template repos in the conversion log (`conversion-log.md` — to be created).
9. **Delete** absorbed template repos (both GitHub and local clone) — only after successful validation and local install.

### Marketplace update (after all clusters are converted)

1. Remove `new-repo-from-template` from `marketplace.json`.
2. Add any newly-created cluster plugins.
3. Bump versions for extended existing plugins.
4. Commit + push `Claude-Code-Plugins`.

### Local refresh

1. On this workstation, `claude plugin update` (or equivalent) so the new cluster plugins are installed/refreshed.
2. Smoke-test at least one `new-workspace` skill from `~` to confirm the VAD-style pain point is resolved.

### Index update + docs site redeploy

1. Update [`Claude-Code-Projects-Index`](https://github.com/danielrosehill/Claude-Code-Projects-Index) (local stale clone at `~/repos/github/my-repos/Claude-Code-Repos-Index/` — rename dir to match) — substantial update:
   - Remove entries for every deleted standalone template repo.
   - Add entries for all 27 new/extended cluster plugins with Title-Case names and new description format.
   - Update counts, categorization, and any screenshots/tables.
2. Commit + push the index repo.
3. **Redeploy the docs site** generated from the index repo (find the deployment mechanism — likely a Pages/Workers setup; check for `.github/workflows/` or a `vercel.json`/`wrangler.toml`).

### Retire New-Repo-From-Template

1. After all clusters are live and the index is updated, archive (not delete) `New-Repo-From-Template-Plugin` on GitHub as historical reference. The plugin is removed from the marketplace but the repo persists read-only.

---

## 4. Open items to resolve before execution

These are not blockers for clustering but must be decided before step 3 per-cluster conversion starts:

- **Default workspace location.** Locked: `~/repos/github/my-repos/<workspace-name>`. Skill accepts an override.
- **GH repo creation by default?** Locked: yes, public, with `--local-only` opt-out.
- **Sub-variant argument syntax.** `$ARGUMENTS` parsed with the first token as workspace name, subsequent tokens as flags. Variants passed as `--variant=<name>`.
- **Naming collisions** across plugins. Every plugin's provisioning skill is named `new-workspace`; accessed as `/<plugin-name>:new-workspace`. No collision risk.

---

## 5. Task handoff

Per Daniel's request: after this plan, individual conversion tasks are handed out one at a time. The task list reflects this — see `TaskList` for the per-cluster tickets once spawned.
