# Hypothetical Usage Example

Grounding example of how Daniel actually uses these templates day-to-day, and how the updated model should change the feel of it.

## The scenario

Daniel wants to start a new research project on Israeli OSINT sources. He opens Claude Code at home (`~`, not inside any project), turns on bypass-permissions, and types:

> Let's create a new research plugin called Israel-OSINT-Planner (public). Template: public research space.

He expects: a fresh workspace to exist on disk, pre-seeded with the research scaffold (CLAUDE.md, `context/`, `outputs/`, research-specific slash commands), pushed to GitHub as a public repo, ready to start filling in.

## Under the current model (New-Repo-From-Template)

1. The `new-repo-from-template` plugin is globally installed, so the skill is reachable from `~`.
2. The skill looks up "public research space" in its bundled manifest of 111 scaffolds.
3. It clones the template repo from GitHub, renames the directory, pushes as a new repo.
4. The new workspace contains its *own* `.claude/commands/`, `.claude/agents/`, `.claude/skills/` — research-specific primitives live inside the workspace.

Works. But two issues:

- To use the research primitives (e.g. a `/source-log` command), Daniel has to `cd` into the new workspace. The primitives aren't available from `~` or from a sibling repo.
- Every new workspace carries its own copy of those primitives. When he improves the `/source-log` command, old workspaces don't get the update.

## Under the updated model (plugin-per-cluster)

1. A `research-space` plugin is installed from the marketplace. It contains:
   - The research primitives as plugin-level skills/commands (globally available everywhere).
   - A bundled scaffold under `template/` (CLAUDE.md, context/, outputs/, README — no `.claude/` tree).
   - A provisioning skill, e.g. `/research-space:new-workspace Israel-OSINT-Planner`.
2. Daniel runs the provisioning skill from `~`. It:
   - Copies the bundled scaffold to the target path.
   - Personalizes CLAUDE.md using ambient facts from `~/.claude/CLAUDE.md` (user memory).
   - Prompts for any research-specific facts (topic focus, output style).
   - Optionally `gh repo create`s and pushes.
3. The research primitives (`/source-log`, `/summarize-sources`, etc.) are already globally available via the installed plugin — no `cd` required.
4. When Daniel improves `/source-log`, bumping the plugin version propagates the new behavior to every workspace on next `claude plugin update`. The workspace directory itself only holds **data**.

## What this tells us about clustering

- The research-space plugin is one of many such clusters. Each cluster is a domain (research, home-ops, personal-planning, media-production, docs/writing, debugging, etc.).
- The current template manifest probably has 3–6 templates per cluster that differ only in surface framing (e.g. "deep research template", "competitor research agent", "technical research template"). Those should collapse into one plugin with a parameterized scaffold, not three near-duplicate plugins.
- A cluster gets its own plugin when the **primitives** are distinctive. If two templates would ship identical primitives with only the scaffold README differing, they should be one plugin whose `new-workspace` skill takes a sub-variant argument.

## The pivot point (real pain point that triggered this reshape)

The moment the current model broke for Daniel:

He wanted to run **VAD (voice activity detection)** on an audio file living in a different repository — not the audio-engineering workspace itself, some other project he happened to be working in. He knew perfectly well that he had defined a VAD skill inside his audio-engineering workspace. He'd used it before. It worked. But because that skill lived inside a consolidated workspace repo (as `.claude/skills/vad/` or similar), it was only active when Claude's cwd was inside that workspace.

So he had a functional, already-built tool sitting on disk, knew exactly where it was — and couldn't invoke it from the repo where he actually needed it. To use his own skill, he'd have to either:

1. `cd` to the audio workspace, process the file, then copy results back — breaking flow.
2. Duplicate the skill into every workspace that might ever need audio processing — defeating the point of having one authoritative version.

That's when the pattern visibly stopped scaling. Primitives ("do VAD") want to be **globally available** and live in one maintained place. Data ("this specific audio project") wants to be **per-workspace**. Consolidating both into one workspace repo fused them together and broke the primitives' reachability.

The fix is the model above: primitives go in plugins (globally available from any cwd), scaffolds are provisioned as data stores by the plugin. The audio-engineering workspace shouldn't own the VAD skill — an `audio-engineering` plugin should, and it should ship a provisioning skill for a new audio project scaffold on the side.

## Open question this example raises

Should the provisioning skill also handle the `gh repo create` + initial push? The screenshot implies yes (Daniel expects "public plugin"-style framing — i.e. the workspace is born on GitHub, not just on disk). Default to: **yes, create on GitHub**, with a `--local-only` flag to skip when the user doesn't want a remote.
