# agent-skills

[![skills.sh](https://skills.sh/b/alexgenovese/agent-skills)](https://skills.sh/alexgenovese/agent-skills)
![GitHub last commit](https://img.shields.io/github/last-commit/alexgenovese/agent-skills)
![GitHub stars](https://img.shields.io/github/stars/alexgenovese/agent-skills?style=social)
![License](https://img.shields.io/badge/license-MIT-blue)

**Production-grade AI agent skills — portable across Claude Code, Cursor, GitHub Copilot, Codex CLI, and every major coding agent.**

Stop writing one-shot prompts. Ship composable, version-controlled skills that your AI agents discover and load on demand. Each skill stays under 500 lines; deep knowledge lives in `references/`, and agents only pay for what they use.

---

## Features

- **Cross-tool compatible** — same `SKILL.md` format works in Claude Code, Cursor, VS Code Copilot, Codex CLI, Amp, Goose, and Windsurf
- **Zero context waste** — agents load only `name` + `description` at startup; full content injects on invocation ([progressive disclosure](https://gotranscript.com/public/agent-skills-practical-way-to-cut-llm-context-bloat))
- **Composable by design** — agents reference skills by name, skills nest under semantic areas, and the hierarchy stays flat enough for `npx skills` auto-discovery
- **Deterministic scripts** — `scripts/` per skill for repeatable automation (migrations, lint fixes, scaffold generators)
- **CI-validated** — every PR validates frontmatter, naming conventions, and directory structure

---

## Quick Start

```bash
# Install the full collection (agent + all skills)
npx skills add alexgenovese/agent-skills

# Run a single skill without installing
npx skills use alexgenovese/agent-skills@nextjs-core | opencode

# Or invoke by name in any agent:
# "Use the skill 'deployment-strategy' to plan this release"
```

---

## Agents

### [nextjs-expert-cto](nextjs-expert-cto/)

Technical CTO for Next.js teams — architecture decisions, delivery pipelines, code quality, and team velocity.

| Skill | Area | Purpose |
|-------|------|---------|
| `technical-decision-making` | Project Management | ADRs, build-vs-buy, framework evaluation |
| `team-velocity` | Project Management | Bottleneck analysis, cycle time, throughput |
| `deployment-strategy` | Test / Delivery / Deploy | Blue-green, canary, feature flags, rollback |
| `quality-gates` | Test / Delivery / Deploy | CI gates, coverage thresholds, incident response |
| `typescript-ecosystem` | Coding / TypeScript | Strict config, branded types, dependency picks |
| `api-design` | Coding / TypeScript | REST/GraphQL, error contracts, pagination, versioning |

---

## How Progressive Disclosure Works

Agent Skills keep context lean by design. At startup the agent sees only a lightweight index — the full payload arrives when needed.

| Level | Mechanism | When |
|-------|-----------|------|
| **1. Default** | Agent loads `name`/`description` for every skill. Full `SKILL.md` + `references/` injected on invocation. | Always on. Zero context tax. |
| **2. Explicit invocation** | `Use the skill "deployment-strategy" to...` or `/<skill-name> <query>` | Force a specific skill, avoid ambiguity. |
| **3. Isolated sub-agent** | `context: fork` in frontmatter runs the skill in a separate context; returns only the result. | Long-running or destructive tasks. |
| **4. Scoped tools** | `allowed-tools:` limits which tools the skill can call. | Sandboxing, security, focus. |
| **5. External orchestrator** | Policy engine (Agent RuleZ, MCP Optimizer) injects the right skill based on event triggers. | Complex team workflows, skill amnesia in long sessions. |

---

## Directory Structure

```
agent/                          # Top-level agent directory
  SKILL.md                      # Agent definition: name, description, skills list, system prompt
  skills/
    <area>/<skill-name>/        # Skill organized by semantic area
      SKILL.md                  # Skill metadata + instructions (≤ 500 lines)
      scripts/                  # Executable automation (.py, .js, .sh)
      references/               # Deep reference docs loaded on demand (.md, .txt, .pdf)
```

Hierarchy: `agent > skills > scripts, references`

---

## Context Bloat Checklist

- [ ] Every skill stays under 500 lines; bulk goes in `references/`
- [ ] `AGENTS.md` lists only skills relevant to the current project
- [ ] `context: fork` set for tasks exceeding 10 steps or using destructive tools
- [ ] `allowed-tools` is as narrow as the skill allows
- [ ] Long sessions reset context (`/clear` or fresh terminal) between unrelated tasks

---

## License

MIT. Use it, fork it, ship it.
