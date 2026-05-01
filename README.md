# 🎼 Maestro AI

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Node.js](https://img.shields.io/badge/Node.js-18%2B-green.svg)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue.svg)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19-61dafb.svg)](https://reactjs.org/)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow.svg)](https://python.org/)
[![Playwright](https://img.shields.io/badge/Playwright-1.58%2B-green.svg)](https://playwright.dev/)
[![Claude Code](https://img.shields.io/badge/Claude_Code-compatible-orange.svg)](https://claude.ai/code)

**Maestro AI** is a multi-agent orchestration framework built for [Claude Code](https://claude.ai/code). It lets you create, configure, and run teams of specialized AI agents — called **squads** — that automate complex business workflows: content production, research pipelines, data analysis, publishing automation, and more.

---

## What is Maestro AI?

Maestro AI is a framework that lets you:

- **Design squads** — teams of AI agents with distinct roles (researcher, writer, reviewer, designer, publisher) that collaborate through automated pipelines
- **Run pipelines** — squads execute step-by-step workflows with checkpoints, quality gates, and veto conditions that ensure high-quality output every time
- **Integrate skills** — extend squads with installable skills that connect to external services (Apify for scraping, Canva for design, Instagram for publishing, Resend for email, and more)
- **Investigate profiles** — before building a squad, the Sherlock investigation agent can analyze real social media profiles to extract content patterns, hooks, and styles that inform your agents
- **Watch it work** — a real-time dashboard built with React and Phaser shows your agents working in a 2D virtual office, with live status updates via WebSocket

Maestro AI is designed to be used entirely within Claude Code via simple slash commands like `/maestro create` and `/maestro run`.

---

## Architecture

```
maestro-ai/
│
├── _maestro/                       # Maestro AI core (do not modify manually)
│   ├── config/
│   │   └── playwright.config.json  # Playwright browser config
│   ├── core/
│   │   ├── best-practices/         # Platform & format best-practice guides
│   │   │   ├── _catalog.yaml       # Index of available best-practices files
│   │   │   ├── instagram-feed.md
│   │   │   ├── copywriting.md
│   │   │   └── ...
│   │   ├── prompts/                # Agent phase prompts
│   │   │   ├── discovery.prompt.md # Phase 1: Squad wizard
│   │   │   ├── design.prompt.md    # Phase 3: Architecture
│   │   │   ├── build.prompt.md     # Phase 4: File generation
│   │   │   ├── sherlock-*.md       # Phase 2: Profile investigation
│   │   │   └── ...
│   │   ├── architect.agent.yaml    # Architect agent definition
│   │   ├── runner.pipeline.md      # Pipeline runner instructions
│   │   └── skills.engine.md        # Skills engine instructions
│   ├── _browser_profile/           # Persistent browser sessions (gitignored)
│   ├── _investigations/            # Sherlock investigation outputs
│   ├── _memory/
│   │   ├── company.md              # Persistent company context
│   │   └── preferences.md          # User preferences
│   └── logs/                       # Execution logs (gitignored)
│
├── squads/                         # Your squads live here
│   └── {squad-name}/
│       ├── squad.yaml              # Squad definition
│       ├── squad-party.csv         # Agent manifest
│       ├── agents/                 # Agent .agent.md files
│       │   └── {agent}.agent.md
│       ├── pipeline/
│       │   ├── pipeline.yaml       # Pipeline entry point
│       │   ├── steps/              # Step files
│       │   └── data/               # Reference materials
│       ├── output/                 # Generated content (gitignored)
│       │   └── {run-id}/           # Per-run outputs with versioning
│       └── _memory/
│           ├── memories.md         # Persistent squad learnings
│           └── runs.md             # Run history
│
├── skills/                         # Installed skills
│   ├── apify/SKILL.md
│   ├── canva/SKILL.md
│   ├── instagram-publisher/SKILL.md
│   ├── resend/SKILL.md
│   ├── image-ai-generator/SKILL.md
│   └── maestro-skill-creator/      # Tool for creating new skills
│
├── dashboard/                      # Real-time virtual office (React + Phaser)
│   ├── src/
│   │   ├── App.tsx
│   │   ├── office/                 # 2D office scene (Phaser)
│   │   ├── hooks/                  # WebSocket hooks
│   │   └── store/                  # Zustand state
│   └── package.json
│
├── setup/
│   └── maestro-skill/SKILL.md      # Maestro skill — copy to .claude/skills/maestro/
│
├── .env.example                    # Environment variables template
├── .gitignore
├── package.json
├── CLAUDE.md                       # Claude Code instructions
└── README.md
```

---

## Getting Started

### Prerequisites

- [Claude Code](https://claude.ai/code) installed and configured
- [Node.js](https://nodejs.org/) 18 or higher
- [Python](https://python.org/) 3.9 or higher (optional, for script-based skills)
- Git

### Installation

**1. Clone the repository**

```bash
git clone https://github.com/otaviopontes/maestro-ai.git
cd maestro-ai
```

**2. Install dependencies**

```bash
npm install
```

**3. Configure your environment**

```bash
cp .env.example .env
```

Open `.env` and fill in the credentials for the skills you want to use. You only need to configure the services your squads will use.

**4. Install the Maestro skill in Claude Code**

Copy the Maestro skill file to your Claude Code skills directory:

```bash
# macOS / Linux
mkdir -p ~/.claude/skills/maestro
cp setup/maestro-skill/SKILL.md ~/.claude/skills/maestro/SKILL.md

# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\maestro"
Copy-Item setup\maestro-skill\SKILL.md "$env:USERPROFILE\.claude\skills\maestro\SKILL.md"
```

**5. Configure MCP servers (optional)**

If you plan to use skills that require MCP servers (Apify, Canva, etc.), create a `.mcp.json` file in the project root:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--config", "_maestro/config/playwright.config.json"]
    },
    "apify": {
      "command": "npx",
      "args": ["-y", "@apify/actors-mcp-server@latest"],
      "env": {
        "APIFY_TOKEN": "${APIFY_TOKEN}"
      }
    }
  }
}
```

**6. Open the project in Claude Code and start onboarding**

```bash
claude .
```

Then in Claude Code:

```
/maestro
```

The onboarding wizard will ask for your company name, website, and preferences to create your `_maestro/_memory/company.md` profile.

---

## Creating Your First Squad

```
/maestro create "Weekly trend research and report for my niche"
```

Maestro AI guides you through a multi-phase wizard:

**Phase 1 — Discovery:** Up to 8 questions to understand your goal, audience, output format, and frequency.

**Phase 2 — Investigation (optional):** Share Instagram, YouTube, LinkedIn, or Twitter/X profile URLs and the Sherlock agent will analyze real content to extract patterns that inform your agents.

**Phase 3 — Design:** Research your domain, design the agent team, and present a squad architecture for your approval:

```
I'll create a squad with 3 agents:

1. 🔍 Rafael Radar — Trend Researcher
   Tasks: find-trends → rank-by-relevance
2. 📊 Alice Análise — Insights Analyst
   Tasks: generate-report
3. ✅ Vera Veredito — Quality Reviewer
   Tasks: review-report

Pipeline: checkpoint → [Rafael] → checkpoint → [Alice] → [Vera] → checkpoint
```

**Phase 4 — Build:** Generates all squad files with full agent personas, pipeline steps, veto conditions, quality criteria, and reference materials.

### Running Your Squad

```
/maestro run my-squad
```

The pipeline executes step by step. At each checkpoint, Maestro AI asks for your input before proceeding.

---

## Available Skills

| Skill | Type | Description | Required Env Vars |
|-------|------|-------------|-------------------|
| `apify` | MCP | Scrape any website, social media profiles, and search engines | `APIFY_TOKEN` |
| `canva` | MCP | Create and export Canva designs from templates | `CANVA_API_KEY` |
| `instagram-publisher` | Script | Publish carousel posts to Instagram via Graph API | `INSTAGRAM_ACCESS_TOKEN`, `INSTAGRAM_USER_ID`, `IMGBB_API_KEY` |
| `resend` | MCP | Send transactional and marketing emails | `RESEND_API_KEY` |
| `image-ai-generator` | Script | Generate images using DALL-E, Stability AI, or FAL.ai | `IMAGE_AI_PROVIDER`, `IMAGE_AI_API_KEY` |
| `maestro-skill-creator` | Prompt | Create, test, and benchmark new custom skills | — |

### Installing a skill

```
/maestro install apify
```

### Creating a custom skill

```
/maestro skills → Create a custom skill
```

---

## Dashboard

The virtual office dashboard shows your agents working in real time in a 2D office built with Phaser.

```bash
npm run dashboard
# Opens at http://localhost:5173
```

**Tech stack:** React 19 + Vite + Phaser 3 + Zustand + WebSocket

---

## All Commands

| Command | Description |
|---------|-------------|
| `/maestro` | Open the main menu |
| `/maestro help` | Show all commands |
| `/maestro create` | Create a new squad |
| `/maestro list` | List all squads |
| `/maestro run <name>` | Run a squad's pipeline |
| `/maestro edit <name>` | Modify an existing squad |
| `/maestro delete <name>` | Delete a squad |
| `/maestro skills` | Browse installed skills |
| `/maestro install <name>` | Install a skill from catalog |
| `/maestro uninstall <name>` | Remove a skill |
| `/maestro show-company` | Display company profile |
| `/maestro edit-company` | Update company profile |
| `/maestro settings` | Change language and preferences |
| `/maestro reset` | Reset all configuration |

---

## How It Works (Architecture Deep Dive)

### Squad Execution Flow

```
/maestro run my-squad
  │
  ├─ Load squad.yaml + squad-party.csv + company.md + memories.md
  ├─ Resolve skills (fail fast if any skill is missing)
  ├─ Initialize state.json (dashboard updates)
  └─ For each pipeline step:
       ├─ Update dashboard state
       ├─ Validate input file (bash gate)
       ├─ Execute (subagent / inline / checkpoint)
       ├─ Validate output file (bash gate)
       ├─ Check veto conditions
       └─ Handoff to next agent
```

### Agent Context Composition

```
Agent (.agent.md)
  + Platform Best Practices (_maestro/core/best-practices/{format}.md)
  + Skill Instructions (skills/{skill}/SKILL.md body)
```

### Output Versioning

Every output is stored under a run-scoped, versioned path:

```
squads/{squad}/output/{run-id}/v{N}/{filename}
```

Example: `squads/my-squad/output/2026-05-01-143022/v1/report.md`

---

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on reporting bugs, submitting pull requests, adding skills, and improving agent prompts.

---

## License

Copyright 2026 Otávio Pontes

Licensed under the [Apache License 2.0](LICENSE).
