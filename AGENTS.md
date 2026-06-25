# OpenCode Agent Config

This repository hosts AI agents with composable skills organized by competence area, following the [Agent Skills](https://agentskills.io) standard — compatible with Claude Code, Cursor, GitHub Copilot, Codex CLI, Amp, Goose, Windsurf, and Devin.

## Directory Structure

```
agent/                     # Top-level agent directory
  SKILL.md                 # Agent metadata + system prompt
  skills/
    <area>/<skill-name>/   # Skills organized by semantic area
      SKILL.md             # Skill metadata + instructions
      scripts/             # Executable automation (.py, .js, .sh)
      references/          # On-demand reference docs (.md, .txt, .pdf)
```

## Writing Skills

- Each skill lives under `skills/<area>/<name>/SKILL.md`
- Frontmatter requires: `name`, `description`
- SKILL.md must stay under 500 lines; extend with `references/`
- Use imperative tone, explain *why* not just *what*
- `scripts/` holds automation for repetitive or deterministic tasks
- `references/` holds supporting documentation loaded on demand

## Naming Conventions

- Skill directory: kebab-case
- Frontmatter `name` field: must match directory name
- Area name: single word, lowercase (e.g. `product`, `foundation`)

## Validation

Before committing:
- Every SKILL.md must have `name` and `description` in frontmatter
- Every SKILL.md must have `scripts/` and `references/` directories (can be empty)
