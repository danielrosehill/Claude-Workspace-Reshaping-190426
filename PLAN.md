# Claude Workspace Reshaping — Task Definition

> Note: lightly formatted from a voice-typed transcript.

## Grounding / References

- **Marketplace:** https://github.com/danielrosehill/Claude-Code-Plugins
- **Consolidation plugin to unwind:** https://github.com/danielrosehill/New-Repo-From-Template-Plugin — folded many previously-standalone plugins in as repo templates, and also absorbed many templates that were never shaped as plugins to begin with.

## Background

When I began using Claude Code, I felt intuitively that its main use stretched far beyond code generation. I began using it very efficiently for desktop operations, server management, etc. I realized that to support the type of workflow pattern I work around, a folder structure was still useful. A folder structure gives Claude a defined place to store outputs and gives me a defined place to provide context data. So using a combination of folders and code primitives like slash commands and subagents, I was able to define workspaces.

I call these Claude spaces. An example would be Home Assistant Manager — a repository template that the user can clone, shipping with slash commands, the defined structure, and CLAUDE.md files. It's essentially a quick-start. I open-source these publicly to contribute the templates for anyone who wants to use them. In practice, I would first create my public template, and then if it was a private project, I would simply create a private repository from that public template.

Over time, Claude Code has developed quickly and introduced plugins as a mechanism for bundling elements such as slash commands, subagents, and now agent skills, which are incredibly useful. I've been thinking about how to update my collection correspondingly. Plugins continue to provide a lot of use, because by having a public marketplace, I can create plugins that I can then sync onto my laptop or another computer. The only challenge is creating private templates — for that, I'd need a private marketplace.

But a lot of the times where I do that, the cases where it's truly necessary are actually edge cases. More often than not, a clean mechanism to separate user data from the template is a more elegant and practical solution.

My first refactoring was to create a plugin called "create new repository from a template" and nest all my templates within that. My thought was rationalization, but when trying to use my own plugins, I realized there's a deficit: I can only use that plugin to start a workspace from within the place where the skills and subagents live. I need to be using Claude within that defined part of the filesystem in order to have access to the tools. This really reduces the fluidity in the way I try to use Claude, which is maybe unconventional but works best when bypass permissions are on — the agent is free to roam the filesystem, and there's no tight binding between the current CLI position and what tools are available. The original limitation was due to trying to avoid context bloat, but that problem has been essentially overcome by more selective tool calling, rendering it unnecessary.

Today, I thought of a solution that would reduce the need to maintain duplicate elements for each project. If I have a plugin like Desktop Manager that also maintains a template called Desktop Manager Workspace, I could have created a plugin that defines a skill to clone the scaffold onto the user's computer. But I realized an easier way: just ship a single plugin and include the scaffold as data, then define a skill that provisions that scaffold/template which is already bundled.

Claude defines plugin data stores as an innovative mechanism — it can also use that to store user data. Given that the entire objective of many of these plugins is to provide a seamless cross-device experience with Claude, I think there is independent value in allowing the plugins to let the user define an onboarding cue where they'd like to provision or create the workspace.

## Design principle: plugins are generic, data stores are personal

Plugins published to the marketplace **must contain no workspace-specific context** — no Daniel-specific paths, hostnames, identifiers, or preferences. They ship only the generic behavior (skills, subagents, commands) plus a generic scaffold.

The standard pattern every plugin should follow:

1. **Onboarding command/skill** — first-run setup that:
   - Asks the user where they want the data store provisioned (or accepts it as an argument).
   - Creates the scaffold at that path.
   - Reads `~/.claude/CLAUDE.md` (user memory) to pick up OS, locale, timezone, and other ambient facts, and personalizes the newly created workspace accordingly.
   - Prompts for any plugin-specific facts it can't infer and writes them into the workspace's own CLAUDE.md / context files.
2. **Stable variable references** — the plugin resolves "the data store" via a configured path (stored in `${CLAUDE_PLUGIN_DATA}` or a user-config file the onboarding skill writes), and resolves "the user" via `~/.claude/CLAUDE.md`. All plugin commands read from those references rather than hard-coding anything.
3. **Forward compatibility** — because personalization lives in the user's data store (not in the plugin), new skills/commands can be added to the plugin in later versions without breaking existing users' setups.

### Cross-device preference sync (open question)

If a user wants their personalization to follow them across machines, the plugin itself shouldn't solve this — it's out of scope. Options the user can pick:
- Version-control the data store in a private git repo.
- Keep it in a synced cloud folder.
- Maintain a user-owned private "preferences" repo/gist that the onboarding skill optionally pulls from.

The plugin's responsibility ends at *reading* from a configured data-store path; *how* that path stays in sync across devices is a user concern.

## Objective

Go through my plugin marketplace with a view to refactoring again and consolidating according to this new pattern:

1. Remove the "create new repository from template" plugin entirely.
2. Break out each template that's subsumed into it back into its own plugin, as originally structured.
3. Take the corresponding templates, where they existed, and nest them within those plugins.
4. Delete the now-redundant standalone template repositories.

---

## Plan Review (grounded against Claude Code docs in `docs/`)

### What the docs confirm

1. **The plan correctly separates primitives from data.** Today, New-Repo-From-Template provisions a workspace whose `.claude/` directory holds the slash commands / skills / subagents — so those primitives are **project-scoped** (only active when cwd is inside the provisioned repo). Breaking each domain out into its own plugin makes the primitives **global** (installed once via the marketplace, available from any cwd), while the plugin's provisioning skill still lays down a **data/context scaffold** wherever the user wants it on disk. That's the "best of both worlds" — the primitives detach from the data directory, which is exactly what the plugin model is built for.

2. **Bundling a scaffold as data inside a plugin is supported.** Plugins are copied to `~/.claude/plugins/cache/<marketplace>/<plugin>/<version>/` at install time. Bundled template files ride along.

3. **The right variable for a provisioning skill is `${CLAUDE_PLUGIN_ROOT}`**, not `${CLAUDE_SKILL_DIR}`. `CLAUDE_SKILL_DIR` points at the skill's own subdirectory (e.g. `<plugin>/skills/provision/`); `CLAUDE_PLUGIN_ROOT` points at the plugin root, which is where you'd stash the scaffold (e.g. `<plugin>/template/`). Use `CLAUDE_PLUGIN_ROOT` in hooks and MCP configs. In skill bash injection (`` !`cmd` ``), `CLAUDE_PLUGIN_ROOT` is only exported in hooks/MCP contexts — from a skill, walk up from `${CLAUDE_SKILL_DIR}/..` instead, or keep the template inside the skill folder.

4. **`${CLAUDE_PLUGIN_DATA}` is the correct home for user data** that must survive plugin updates. Good fit for your "seamless cross-device" goal — but note that cross-device sync of that directory is your problem, not Claude Code's. The docs don't promise any sync.

5. **Private marketplaces work.** Host on a private GitHub repo, users set `GITHUB_TOKEN`/`GH_TOKEN` for auto-updates. You already have `danielrosehill-private`. The "private marketplace is painful" framing is less true than it was; the main friction is token management on new machines.

### Where the plan is sound

- Consolidating each (plugin + scaffold) pair into a single plugin is directly supported and is the idiomatic pattern the docs imply for packaging context-heavy workflows.
- A `provision-workspace` skill with `disable-model-invocation: true` + `allowed-tools: Bash(mkdir *) Bash(cp *) Bash(git init *)` is the right shape.
- Separating "template data bundled in plugin" from "user data in `${CLAUDE_PLUGIN_DATA}`" maps cleanly onto the docs.
- Removing the umbrella "New-Repo-From-Template" plugin is reasonable once each template is self-contained.

### Implication: the data scaffold should be minimal

Once primitives live in the plugin, the provisioned workspace is *just data* — CLAUDE.md, `context/`, `outputs/`, maybe a README. No `.claude/commands/`, no `.claude/agents/`, no `.claude/skills/` (those would shadow the plugin's globals and re-create the old problem). The provisioning skill should deliberately **not** copy any `.claude/` tree into the target. If you want per-workspace overrides, keep them as CLAUDE.md content, not as primitive duplicates.

### Concerns / things to decide before executing

1. **What do you lose by removing New-Repo-From-Template?** It has discovery value — one place to list "all the workspace templates I have." After the split you'll want either:
   - a single "meta" skill in each plugin that advertises what scaffold it provisions, plus a marketplace-level description convention, or
   - a lightweight "catalog" plugin that only documents *which* other plugins provision which scaffolds (no templates nested).
   Decide which before deleting the old plugin.

2. **Templates that were never plugins.** You said New-Repo-From-Template absorbed templates that were never shaped as plugins. For each of those, the split requires *creating a new plugin from scratch* to host the scaffold. That's more work than the plan implies. Candidates:
   - Is the template substantial enough to justify a plugin (skills/agents/hooks)?
   - Or is it just a scaffold with no behavior? → In that case a plugin is overkill; keep it as a standalone template repo and have the user `gh repo create --template` it. Don't force everything into the plugin shape.

3. **Don't delete the standalone template repos yet.** Step 4 is a one-way action. Suggest: after each plugin is published and validated with `claude plugin validate`, archive (not delete) the old template repo for 30 days before removing. Also check: are any of them referenced by existing private repos created from the template? GitHub's "created from template" link persists but isn't load-bearing.

4. **Version/update story for bundled scaffolds.** When you update the bundled template in the plugin, users who already provisioned a workspace won't get the update (by design — user data). You may want a `resync-workspace` skill that diffs the bundled scaffold against a provisioned workspace and offers to pull specific files forward.

5. **Naming collision.** Plugin skills are namespaced as `/plugin-name:skill-name`. If every plugin gets a `provision` skill, it becomes `/home-assistant-manager:provision`, `/desktop-manager:provision`, etc. Fine, but pick the skill name deliberately — something like `new-workspace` may read better than `provision`.

6. **`strict: false` in marketplace entries is a footgun.** Keep `strict: true` (default) and let each plugin's own `plugin.json` be authoritative. The consolidation doesn't require strict mode changes.

### Suggested execution order (revised)

1. Pick **one** plugin from the current set (e.g. Home-Assistant-Manager) as the pilot.
2. Add the scaffold into the plugin as a `template/` directory.
3. Add a `skills/new-workspace/SKILL.md` using `${CLAUDE_SKILL_DIR}/../../template` + `$ARGUMENTS` for target path.
4. Validate: `claude plugin validate .` → bump version → publish via marketplace → install fresh on a second machine → provision from a random cwd. Confirm fluidity problem is actually gone.
5. Only once that works end-to-end, repeat for the rest.
6. For templates-that-were-never-plugins, decide per-item: promote to plugin vs. keep as standalone template repo.
7. Remove New-Repo-From-Template last. Leave old template repos archived for a cooling period.

### Open questions for you

- Is there a preferred location for provisioned workspaces (`~/repos/github/my-repos/`?) that the skill should default to, or always prompt?
- Should each plugin's provisioner also offer to create the GitHub repo (using `gh`)?
- What's the granularity for which templates become plugins vs stay as template repos? Rough rule: "has any Claude behavior attached" → plugin; "pure folder scaffold" → template repo.

