# Publishing Loop Engineering

This repository is shaped for GitHub plus the open skills ecosystem.

## What the current ecosystem expects

- `skill.sh` is currently a parked domain; the active public directory is `skills.sh`.
- Skills are GitHub-hosted folders containing `SKILL.md` with YAML frontmatter.
- `skills.sh` is a discovery and leaderboard surface, not a manual upload form.
- The `skills` CLI installs from GitHub and reports anonymous aggregate telemetry.
- A repository appears on `skills.sh` after it has been installed with `npx skills add`.
- `skills.sh.json` customizes how a repository page is grouped after the repo is listed.

Sources checked:

- https://skills.sh/
- https://skills.sh/docs
- https://skills.sh/docs/cli
- https://skills.sh/docs/customize
- https://skills.sh/docs/faq

Popular public skill repos checked for positioning:

- https://github.com/vercel-labs/skills
- https://github.com/mattpocock/skills
- https://github.com/obra/superpowers
- https://github.com/Leonxlnx/taste-skill

Observed README patterns:

- Lead with a sharp promise, not a taxonomy.
- Put install commands in the first screen.
- Explain the repeated pain the skill removes.
- Include a compact skill table when the repo has one or more skills.
- Show examples before deep docs.
- State compatibility and safety assumptions plainly.
- Keep marketplace/docs content at the repo root, not inside the runtime `SKILL.md`.

## Recommended launch path

1. Create a public GitHub repository named `loop-engineering`.
2. Copy this package into the repository root.
3. Confirm the README install commands use the real GitHub owner/repo.
4. Keep the runtime skill under `skills/loop-engineering/`.
5. Install once with:

   ```bash
   npx skills add https://github.com/parkavenue9639/loop-engineering --skill loop-engineering
   ```

6. Verify it can install for Codex:

   ```bash
   npx skills add https://github.com/parkavenue9639/loop-engineering --skill loop-engineering -a codex
   ```

7. Add GitHub topics:

   ```text
   agent-skills, codex, claude-code, cursor, ai-agents, goal-management, subagents, workflow, skills-sh
   ```

8. Share with a concrete demo, not an abstract claim:

   ```text
   I made a tiny agent skill that prevents fuzzy goal-mode starts.
   It forces a visible Goal Preview before create_goal, including done signals,
   verification, delegation, and stop rules.
   ```

## README positioning

Use a direct pain statement:

```text
Stop launching fuzzy long-running agents.
```

Then show the loop:

```text
request -> goal spec -> ready gate -> visible preview -> create goal -> execute
```

This mirrors the style of popular skill repositories: short promise, immediate install command, concrete examples, compatibility notes, and a clear skill table.

## Marketplace notes

`skills.sh` ranks by anonymous install telemetry. The practical growth loop is:

1. Publish a clear GitHub repo.
2. Make the install command copy-pasteable.
3. Get initial users to install through `npx skills add`.
4. Keep the skill narrow and safe so audit systems and users can inspect it quickly.
5. Update the README with real examples as people use it.

## Compatibility notes

This skill is most useful in agents that support:

- Agent Skills / `SKILL.md` packages.
- Goal, task, plan, or long-running execution modes.
- Optional subagents or external review agents.

It is designed around Codex goal mode, but the core pattern is portable to Claude Code, Cursor, and other agents that can interpret skill instructions.

## What not to do

- Do not put marketing copy inside `skills/loop-engineering/SKILL.md`.
- Do not add scripts unless there is a deterministic check worth automating.
- Do not imply that the skill can create goals in agents that have no goal API.
- Do not make external delegation the default.
- Do not hide the safety model; "instruction-only, no scripts" is a selling point.
