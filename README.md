# 🚀 Agent Skills Library

A high-performance, composable library of AI agent skills following the [Agent Skills Specification](https://agentskills.io). This repository provides a modular architecture for AI agents to specialize in high-level technical leadership, specifically tailored for **Next.js**, **TypeScript**, and **Modern Web Architecture**.

## 🛠️ Architecture

This library uses a **Composable Skill Architecture**. Instead of one massive system prompt, capabilities are split into atomic "skills" that can be dynamically loaded by an orchestrator agent.

### Structure
- `skills/`: Contains specialized capabilities. Each folder contains a `SKILL.md` with YAML frontmatter for discovery.
- `AGENTS.md`: The global configuration and standards for the repository.
- `nextjs-expert-cto`: A high-level orchestrator agent that composes multiple lower-level skills to act as a Technical Leader.

---

## 🌟 Featured Agent: Next.js Expert CTO

The `nextjs-expert-cto` is a CTO-level AI agent designed to lead technical strategy and ensure excellence in Next.js projects.

### Core Capabilities
The CTO agent orchestrates the following specialized skills:

| Skill | Focus | Example Use Case |
| :--- | :--- | :--- |
| **Technical Decision Making** | Trade-off analysis & RFCs | *"Should we use Server Actions or a dedicated API route for this feature?"* |
| **Team Velocity** | DevX & Bottleneck removal | *"Our PR review cycle is too slow; how can we optimize the workflow?"* |
| **Deployment Strategy** | CI/CD & Zero-downtime | *"Design a deployment pipeline for a multi-region Vercel setup."* |
| **Quality Gates** | Testing & Linting standards | *"Define the quality gates for our production-ready TypeScript components."* |
| **TypeScript Ecosystem** | Type safety & Design patterns | *"How do we implement a strictly typed event bus in this architecture?"* |
| **API Design** | Schema & Contract design | *"Design a scalable API contract for our new e-commerce checkout flow."* |
| **Cache Components Adoption** | Strategic caching patterns | *"Determine which parts of the app should use PPR (Partial Prerendering)."* |
| **Cache Components Optimizer** | Performance tuning | *"Optimize the data fetching strategy to reduce TTFB for the product pages."* |
| **Next Dev Loop** | Fast iteration cycles | *"Optimize our local development experience to reduce rebuild times."* |

### 🚀 How to Use

#### For Claude Code / Cursor / Windsurf
1. **Index the Repository**: Point your AI agent to this directory.
2. **Activate the CTO**: Reference the `skills/nextjs-expert-cto/SKILL.md` file.
3. **Prompt**:
   - *"Act as the `nextjs-expert-cto` and review my current architecture."*
   - *"Using the `technical-decision-making` skill, create an RFC for migrating to the App Router."*

#### For `npx skills`
Run the following command to discover skills in this repo:
```bash
npx skills .
```

---

## 📁 Repository Layout

```text
.
├── AGENTS.md               # Global standards & Agent Config
├── LICENSE                 # Project License
├── README.md               # This guide
└── skills/
    ├── nextjs-expert-cto/  # Orchestrator Agent
    │   └── SKILL.md        # System prompt & Skill mapping
    └── <specialized-skill>/ # Atomic capabilities
        ├── SKILL.md        # Logic & Instructions
        ├── references/      # Domain-specific knowledge
        └── scripts/         # Automation tools
```

## 🤝 Contributing

1. **Create a new skill**: Add a folder in `skills/<skill-name>/`.
2. **Define the skill**: Create a `SKILL.md` with the required YAML frontmatter (`name`, `description`).
3. **Add references**: Place any supporting `.md` or `.pdf` files in the `references/` folder.
4. **Link to Agent**: Add the skill name to the `skills:` list in the `nextjs-expert-cto`'s `SKILL.md`.

---
*Powered by the [Agent Skills](https://agentskills.io) standard.*
