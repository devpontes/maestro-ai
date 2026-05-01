# Contributing to Maestro AI

Thank you for your interest in contributing to Maestro AI! This document explains how to report bugs, propose improvements, add new skills, and submit pull requests.

---

## Code of Conduct

By participating in this project, you agree to be respectful and constructive. We want Maestro AI to be a welcoming project for contributors of all backgrounds and experience levels.

---

## Ways to Contribute

There are several ways to contribute to Maestro AI:

- **Report bugs** — open a GitHub Issue with a clear description of what went wrong
- **Request features** — open a GitHub Issue with the label `enhancement` and describe what you'd like to see
- **Improve prompts** — edit files in `_maestro/core/prompts/` or `_maestro/core/best-practices/` to improve agent quality
- **Add skills** — create a new skill under `skills/` following the skill format specification
- **Fix bugs** — fork the repo, make changes on a branch, and submit a Pull Request
- **Improve documentation** — fix typos, improve README clarity, or add missing examples

---

## Development Setup

**1. Fork and clone the repository**

```bash
git clone https://github.com/YOUR_USERNAME/maestro-ai.git
cd maestro-ai
```

**2. Install dependencies**

```bash
npm install
```

**3. Set up your environment**

```bash
cp .env.example .env
# Fill in only the credentials you need for testing
```

**4. Install the Maestro skill in Claude Code**

```bash
# macOS / Linux
mkdir -p ~/.claude/skills/maestro
cp setup/maestro-skill/SKILL.md ~/.claude/skills/maestro/SKILL.md

# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\maestro"
Copy-Item setup\maestro-skill\SKILL.md "$env:USERPROFILE\.claude\skills\maestro\SKILL.md"
```

**5. Open in Claude Code and test**

```bash
claude .
/maestro
```

---

## Pull Request Guidelines

Before submitting a pull request:

- **Create a branch** from `main` with a descriptive name:
  - `feat/add-bluesky-skill` for new features
  - `fix/pipeline-validation-gate` for bug fixes
  - `docs/improve-getting-started` for documentation
  - `prompt/improve-discovery-phase` for prompt improvements

- **Keep PRs focused** — one change per PR whenever possible

- **Test your changes** by running the affected workflows in Claude Code

- **Write a clear PR description** that explains: what you changed, why you changed it, and how to test it

- **Reference related issues** using `Closes #123` or `Related to #456` in your PR description

---

## Adding a New Skill

Skills are the most impactful way to extend Maestro AI. A skill is a `SKILL.md` file in its own directory under `skills/`.

**1. Read the skill format specification**

```
skills/maestro-skill-creator/references/skill-format.md
```

**2. Create your skill directory**

```
skills/my-skill-name/
└── SKILL.md
```

For script-based skills, also add:
```
skills/my-skill-name/
├── SKILL.md
└── scripts/
    └── my-script.py
```

**3. Follow the SKILL.md format**

Every skill must have:
- YAML frontmatter with `name`, `description`, `type`, and `version`
- A clear `## When to use` section
- A step-by-step `## Instructions` section
- A `## Available operations` section (for MCP and script skills)
- An `## Error handling` section

**4. Test the skill**

Install your skill via `/maestro install` and test it in a squad pipeline. Verify that the skill instructions produce correct agent behavior.

**5. Submit a PR**

Include the skill directory in your PR. Skills are reviewed for quality, security (no hardcoded credentials), and usefulness before being merged.

---

## Improving Prompts and Best Practices

The `_maestro/core/prompts/` and `_maestro/core/best-practices/` directories contain the core intelligence of Maestro AI. Improving these files directly improves the quality of all squads built with the framework.

**Guidelines for prompt improvements:**

- Back changes with evidence — if you're improving a prompt, explain what problem it solves or what output quality improvement you observed
- Don't break backward compatibility — existing squads should still work after your change
- Keep prompts clear and specific — vague instructions produce inconsistent agent behavior
- Test with at least one squad before submitting

**Guidelines for best-practices files:**

- Each file covers one platform or content type
- Content should be grounded in real best practices (link sources when possible)
- Include concrete examples, not just abstract advice
- Keep files focused — a best-practices file that covers everything covers nothing

---

## Reporting Bugs

When reporting a bug, please include:

1. **What you did** — the command or action that triggered the bug
2. **What you expected** — what should have happened
3. **What actually happened** — the actual result, including any error messages
4. **Squad configuration** — paste the relevant parts of your `squad.yaml` and step file (remove credentials)
5. **Claude Code version** — run `claude --version` to check

---

## Questions?

If you have questions about contributing, open a GitHub Discussion or reach out via the Issues page.

---

Thank you for helping make Maestro AI better! 🎼
