# Agent Skills

[![skills.sh](https://skills.sh/b/alexgenovese/agent-skills)](https://skills.sh/alexgenovese/agent-skills)

A curated collection of agent skills and agents for OpenCode, Claude Code, Cursor, Codex, and any tool that supports the [Agent Skills](https://agentskills.io) format.

## Structure

```
skills/
  prodotto/             # Product competence
    nextjs-core/        # Next.js App Router core expertise
  foundation/           # Software engineering foundation
    plan/               # Planning & architecture
      task-breakdown/   # Decompose work into increments
      architecture-design/  # System design & ADRs
      time-estimation/  # Realistic effort estimates
    coding/             # Implementation & quality
      code-review/      # Systematic code review
      testing-patterns/ # Tests at every level
      refactoring/      # Structural change, safe execution
agents/                 # Pre-built agent definitions
  software-engineer.md  # Full-stack engineer agent
  nextjs-reviewer.md    # Next.js project reviewer
  code-simplifier.md    # Code quality specialist
```

## Install

```bash
# Install all skills
npx skills add alexgenovese/agent-skills

# Install skills from a specific area
npx skills add alexgenovese/agent-skills --skill nextjs-core

# Use a skill without installing
npx skills use alexgenovese/agent-skills@nextjs-core | opencode
```

> **Nota:** le skill sotto `foundation/plan/` e `foundation/coding/` sono a 3 livelli di profondità. Usa `npx skills add alexgenovese/agent-skills --full-depth` se il tuo client skills non le trova automaticamente.

## Agents

Gli agenti in `agents/` sono file `.md` con frontmatter YAML che referenziano le skill. Possono essere usati come definizioni per agenti in OpenCode, Claude Code, e altri tool compatibili.

| Agent | Descrizione |
|-------|-------------|
| [software-engineer](agents/software-engineer.md) | Pianificazione, architettura, implementazione e revisione |
| [nextjs-reviewer](agents/nextjs-reviewer.md) | Revisione progetti Next.js con auto-fix critici |
| [code-simplifier](agents/code-simplifier.md) | Semplificazione e refactoring di codice |

## Agent Format

```markdown
---
name: agent-name
description: What this agent does
skills:
  - skill-name-1
  - skill-name-2
---
Instructions for the agent...
```

## Skills

| Skill | Area | Description |
|-------|------|-------------|
| [nextjs-core](skills/prodotto/nextjs-core/) | Prodotto | Next.js App Router, caching, Server Components |
| [task-breakdown](skills/foundation/plan/task-breakdown/) | Foundation / Plan | Decomporre il lavoro in task granulari |
| [architecture-design](skills/foundation/plan/architecture-design/) | Foundation / Plan | Decisioni architetturali e ADR |
| [time-estimation](skills/foundation/plan/time-estimation/) | Foundation / Plan | Stime accurate con range di confidenza |
| [code-review](skills/foundation/coding/code-review/) | Foundation / Coding | Revisione strutturata per priorità |
| [testing-patterns](skills/foundation/coding/testing-patterns/) | Foundation / Coding | Testing trophy, async, Next.js |
| [refactoring](skills/foundation/coding/refactoring/) | Foundation / Coding | Refactoring sicuro con characterization test |

## Schema SKILL.md

```markdown
---
name: skill-name
description: What it does and when to trigger
---

# Skill Name

Instructions for the agent...

## References

See `references/file.md` for deeper docs.  <!-- loaded on demand -->
```

Ogni skill può avere `references/`, `scripts/`, o `assets/` per contenuti extra caricati su richiesta.

## License

MIT
