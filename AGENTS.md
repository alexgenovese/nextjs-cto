# OpenCode Agent Config

This repository hosts AI agents with composable skills organized by competence area, following the [Agent Skills](https://agentskills.io) standard — compatible with Claude Code, Cursor, GitHub Copilot, Codex CLI, Amp, Goose, Windsurf, and Devin.

Skills are discoverable at `skills/<name>/SKILL.md` for `npx skills` and referenced by agents via `skills:` list.

## Directory Structure

```
skills/                         # Discovered by npx skills
  <skill-name>/                 # kebab-case, matched to name field
    SKILL.md                    # name + description in YAML frontmatter
    scripts/                    # Executable automation (.py, .js, .sh)
    references/                 # On-demand reference docs (.md, .txt, .pdf)

<agent>/                        # Agent definition directory
  SKILL.md                      # Agent metadata + skills list + system prompt
```

## Writing Skills

- Each skill lives at `skills/<name>/SKILL.md` (repo root)
- Frontmatter requires: `name`, `description`
- `name` must match the parent directory name (kebab-case)
- Description must be clear about when to use the skill (agents activate based on name + description)
- SKILL.md under 500 lines; extend with `references/`
- Use imperative tone, explain *why* not just *what*
- `scripts/` holds automation for repetitive or deterministic tasks
- `references/` holds supporting documentation loaded on demand

## Naming Conventions

- Skill directory: kebab-case
- Frontmatter `name` field: must match directory name

## Validation

Before committing:
- Every SKILL.md must have `name` and `description` in frontmatter
- Every SKILL.md must have `scripts/` and `references/` directories (can be empty)
