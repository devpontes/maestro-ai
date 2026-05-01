# Maestro AI — Project Instructions

This project uses **Maestro AI**, a multi-agent orchestration framework for Claude Code.

---

## Quick Start

Type `/maestro` to open the main menu, or use any of these commands directly:

| Command | Description |
|---------|-------------|
| `/maestro create` | Create a new squad from a natural language description |
| `/maestro run <name>` | Run a squad's pipeline |
| `/maestro list` | List all squads |
| `/maestro edit <name>` | Modify an existing squad |
| `/maestro skills` | Browse and manage skills |
| `/maestro help` | Show all available commands |

---

## Directory Structure

```
maestro-ai/
├── _maestro/                    # Maestro AI core (do not modify manually)
│   ├── config/                  # Playwright browser configuration
│   ├── core/
│   │   ├── best-practices/      # Platform & format guides (used at runtime by agents)
│   │   ├── prompts/             # Phase prompts (discovery, design, build, sherlock)
│   │   ├── architect.agent.yaml # Architect agent
│   │   ├── runner.pipeline.md   # Pipeline runner
│   │   └── skills.engine.md     # Skills engine
│   ├── _browser_profile/        # Persistent browser sessions (gitignored, private)
│   ├── _investigations/         # Sherlock investigation outputs
│   ├── _memory/
│   │   ├── company.md           # Company context (auto-populated by onboarding)
│   │   └── preferences.md       # User preferences (language, defaults)
│   └── logs/                    # Execution logs (gitignored)
│
├── squads/                      # Your squads
│   └── {squad-name}/
│       ├── squad.yaml           # Squad definition (name, domain, skills, pipeline ref)
│       ├── squad-party.csv      # Agent manifest (path, displayName, icon)
│       ├── agents/              # Agent .agent.md files
│       ├── pipeline/
│       │   ├── pipeline.yaml    # Pipeline steps definition
│       │   ├── steps/           # Step files (one per pipeline step)
│       │   └── data/            # Reference materials (research-brief, domain-framework, etc.)
│       ├── output/              # Generated outputs (gitignored, per-run versioned)
│       └── _memory/
│           ├── memories.md      # Persistent squad learnings from past runs
│           └── runs.md          # Run history log
│
├── skills/                      # Installed skills (each has a SKILL.md)
│   ├── apify/
│   ├── canva/
│   ├── instagram-publisher/
│   ├── resend/
│   ├── image-ai-generator/
│   └── maestro-skill-creator/
│
├── dashboard/                   # React + Phaser virtual office (optional)
│
├── setup/
│   └── maestro-skill/SKILL.md   # Install this to .claude/skills/maestro/ to enable /maestro
│
├── .env                         # Credentials (gitignored — copy from .env.example)
├── .env.example                 # Documented environment variables template
├── .gitignore
├── package.json
└── CLAUDE.md                    # This file
```

---

## How It Works

### The Four Phases of Squad Creation

When you run `/maestro create`, Maestro AI orchestrates four phases:

1. **Discovery** (`_maestro/core/prompts/discovery.prompt.md`) — A smart wizard that asks up to 8 questions to understand your goal, audience, tools, and output format. Optionally offers profile investigation.

2. **Investigation** (`_maestro/core/prompts/sherlock-*.md`) — Optional. The Sherlock agent analyzes real social media profiles (Instagram, YouTube, Twitter/X, LinkedIn) to extract content patterns that inform your agents.

3. **Design** (`_maestro/core/prompts/design.prompt.md`) — Researches your domain, designs the agent team architecture, and presents it for your approval. Outputs `squads/{code}/_build/design.yaml`.

4. **Build** (`_maestro/core/prompts/build.prompt.md`) — Generates all squad files from the approved design: agent `.agent.md` files, pipeline steps, reference materials, and runs validation gates.

### Running a Squad

When you run `/maestro run {name}`, the **Pipeline Runner** (`_maestro/core/runner.pipeline.md`):

1. Loads the squad (squad.yaml, squad-party.csv, company.md, memories.md)
2. Resolves all skills (fails fast if any skill is missing or misconfigured)
3. Initializes `state.json` (powers the real-time dashboard)
4. Executes each pipeline step with binary validation gates
5. After completion: archives state, updates squad memories, presents a summary

### Skills

Skills extend squads with external integrations. Each skill lives in `skills/{name}/SKILL.md`.

Skill types: `mcp` (MCP server), `script` (custom script), `hybrid` (both), `prompt` (behavioral instructions only).

The **Skills Engine** (`_maestro/core/skills.engine.md`) manages installation, configuration, and injection of skill instructions into agent context.

### Dashboard

The real-time virtual office dashboard (`dashboard/`) shows agents working in a 2D office with live status. Run with `npm run dashboard` — connects to `state.json` via WebSocket.

---

## Agent File Format

Every agent is a `.agent.md` file with YAML frontmatter + Markdown body:

```markdown
---
id: "squads/{code}/agents/{agent}"
name: "{Agent Name}"
title: "{Agent Title}"
icon: "{emoji}"
squad: "{code}"
execution: inline | subagent
skills: []
tasks:         # optional — list of task files to execute in sequence
  - tasks/task-one.md
---

# Agent Name
## Persona
### Role / ### Identity / ### Communication Style
## Principles
## Operational Framework
## Voice Guidance
## Output Examples
## Anti-Patterns
## Quality Criteria
## Integration
```

---

## Pipeline Step File Format

Every step file has YAML frontmatter + Markdown body:

```markdown
---
execution: subagent | inline
agent: {agent-id}
format: {format-id}         # optional — auto-injects best-practices
inputFile: squads/{code}/output/{file}
outputFile: squads/{code}/output/{file}
model_tier: fast | powerful # only for subagent steps
---

# Step NN: Step Name
## Context Loading
## Instructions
## Output Format
## Output Example
## Veto Conditions
## Quality Criteria
```

Checkpoint steps use `type: checkpoint` instead.

---

## squad.yaml Structure

```yaml
name: "Squad Display Name"
code: "squad-slug"
description: "One-line description"
domain: "content | research | automation | analysis | mixed"
language: "English | Português (Brasil) | ..."
frequency: "Weekly | Daily | On demand"
version: "1.0.0"
created: "2026-05-01"

skills:
  - web_search
  - web_fetch
  - apify            # installed skills

data:
  - pipeline/data/research-brief.md
  - pipeline/data/domain-framework.md
  - pipeline/data/quality-criteria.md
  - pipeline/data/output-examples.md
  - pipeline/data/anti-patterns.md

pipeline: pipeline/pipeline.yaml
memory: _memory/memories.md
runs: _memory/runs.md
output_dir: output/
```

---

## Rules (for Claude Code)

- **Always use `/maestro` commands** to interact with the system — do not manually edit files in `_maestro/core/` unless you know what you're doing
- **Squad YAML files can be edited manually** if needed, but prefer `/maestro edit {squad}`
- **Company context** in `_maestro/_memory/company.md` is loaded for every squad run — keep it accurate
- **Never commit `.env`** — it contains your credentials
- **Browser sessions** in `_maestro/_browser_profile/` are gitignored — they contain login cookies and are private to you
- **The native Claude Code Playwright plugin must be disabled** if you're using investigation features — Maestro AI uses its own `@playwright/mcp` server configured in `.mcp.json`
- **Use the Write tool for file creation** — never use Bash mkdir (avoids Windows/Bash path separator conflicts)
- **Output path transformation** is handled automatically by the pipeline runner — all outputs land under `squads/{name}/output/{run-id}/v{N}/`

---

## First Time Setup

If `_maestro/_memory/company.md` contains `<!-- NOT CONFIGURED -->`, run:

```
/maestro
```

The onboarding wizard will:
1. Ask for your name and preferred language
2. Ask for your company name and website
3. Research your company (WebFetch + WebSearch)
4. Present findings for confirmation
5. Save to `_maestro/_memory/company.md`
6. Show the main menu

---

## Troubleshooting

**`/maestro` command not found** — Make sure you copied `setup/maestro-skill/SKILL.md` to `~/.claude/skills/maestro/SKILL.md`.

**Skill not working** — Run `/maestro skills` to check which env vars are missing. Add them to `.env`.

**Pipeline fails at validation gate** — The step's output file was not created. Check the step file's `outputFile` path starts with `squads/{name}/output/`. Run `/maestro edit {squad}` to fix.

**Dashboard not updating** — Make sure `state.json` is being written by the pipeline runner. Check that `squads/{name}/` is writable.

**MCP server not found** — Ensure `.mcp.json` is in the project root and the skill's `server_name` matches the key in `mcpServers`.
