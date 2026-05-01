# Maestro AI — Skill Format Reference

## SKILL.md Structure

Every Maestro AI skill consists of a `SKILL.md` file with YAML frontmatter and a Markdown body.

---

### Common Frontmatter Fields (all types)

| Field | Required | Description |
|-------|----------|-------------|
| `name` | ✅ | Skill identifier (lowercase, hyphenated, e.g. `my-skill`) |
| `description` | ✅ | What the skill does and when to use it (plain language) |
| `type` | ✅ | `mcp`, `script`, `hybrid`, or `prompt` |
| `version` | ✅ | Semver version string (e.g. `1.0.0`) |
| `categories` | optional | Classification tags array (e.g. `["social-media", "content"]`) |
| `env` | optional | Required environment variable names array |

---

### Type: mcp

MCP skills connect to external APIs via Model Context Protocol servers.

Additional fields under `mcp:`:

| Field | Required | Description |
|-------|----------|-------------|
| `server_name` | ✅ | Key used in `.mcp.json` mcpServers config |
| `command` | ✅ (stdio) | Command to run the server (e.g. `npx`) |
| `args` | ✅ (stdio) | Command arguments array |
| `transport` | ✅ | `stdio` or `http` |
| `url` | ✅ (http) | Server URL for HTTP transport |
| `headers` | optional | Auth headers mapping — key is header name, value is env var name |

---

### Type: script

Script skills run custom code via a specific runtime.

Additional fields under `script:`:

| Field | Required | Description |
|-------|----------|-------------|
| `path` | ✅ | Script path relative to the skill directory |
| `runtime` | ✅ | `node`, `python`, or `bash` |
| `dependencies` | optional | Package list to install before running |
| `invoke` | ✅ | Full invocation command template with `{placeholders}` |

---

### Type: hybrid

Hybrid skills combine both MCP and script components. Include both `mcp:` and `script:` sections as defined above.

---

### Type: prompt

Prompt skills are pure behavioral instructions for agents with no external integration. No additional fields beyond the common fields are required — the Markdown body is the entire skill.

---

## Markdown Body

The body contains instructions for agents. Recommended sections:

- `## When to use` — describes when agents should activate this skill
- `## Instructions` — step-by-step usage guide
- `## Available operations` — what the skill can do (especially for MCP/script types)
- `## Examples` — input/output examples to guide agent behavior
- `## Error handling` — what to do when things go wrong

---

## Complete Examples

### Example: MCP Skill

```yaml
---
name: canva-design
description: Create and manage Canva designs. Use when users want to generate social media graphics, presentations, or any visual content via Canva.
type: mcp
version: 1.0.0
categories:
  - design
  - social-media
env:
  - CANVA_API_KEY
mcp:
  server_name: canva
  command: npx
  args: ["-y", "@canva/mcp-server@latest"]
  transport: stdio
---

# Canva Design Skill

## When to use

Use this skill when the user or squad pipeline needs to create visual content...

## Instructions

1. Understand the design requirements
2. Search for relevant templates
3. Generate the design
4. Present candidates to the user
5. Export in the requested format

## Available operations

- Search templates
- Generate designs
- Export to PNG, PDF, MP4
```

### Example: Script Skill

```yaml
---
name: csv-analyzer
description: Analyze CSV files and generate summary reports.
type: script
version: 1.0.0
categories:
  - data
  - analytics
script:
  path: scripts/analyze.py
  runtime: python
  dependencies:
    - pandas
    - matplotlib
  invoke: python scripts/analyze.py --input {input_file} --output {output_dir}
---

# CSV Analyzer

## When to use

Use this skill when the pipeline needs to analyze tabular data from CSV files.

## Instructions

1. Read the input CSV file to understand its structure
2. Run the analysis script
3. Review the generated report
4. Present findings with key insights highlighted
```

### Example: Prompt Skill

```yaml
---
name: professional-tone
description: Write content in a professional corporate tone.
type: prompt
version: 1.0.0
categories:
  - writing
  - tone
---

# Professional Tone

## When to use

Apply this skill whenever generating content for clients, executives, or external stakeholders.

## Instructions

1. Use formal but accessible language
2. Lead with the conclusion or recommendation
3. Keep sentences concise (15-20 words average)
4. Use active voice by default
5. Structure content with clear headings for scannability
```
