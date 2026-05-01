---
name: canva
description: >
  Create and manage Canva designs using the Canva API.
  Generate social media graphics, presentations, and visual content
  from templates or custom designs.
type: mcp
version: "1.0.0"
mcp:
  server_name: canva
  command: npx
  args: ["-y", "@canva/mcp-server@latest"]
  transport: stdio
env:
  - CANVA_API_KEY
categories: [design, visual, social-media]
---

# Canva Design

## When to use

Use Canva when you need to create visual content using Canva's template library — social media posts, presentations, flyers, thumbnails, or any graphic design task that benefits from Canva's extensive template collection.

## Instructions

### Key capabilities

- Search Canva's template library by keyword, format, or style
- Create new designs from templates or blank canvases
- Edit text, images, colors, and layout elements
- Export designs in PNG, JPG, PDF, or MP4 format

### Workflow

1. Understand the design requirements (type, dimensions, brand colors, content)
2. Search for relevant templates: `search-brand-templates`
3. Generate the design with `generate-design`
4. Present design candidates to the user for selection
5. Export in the requested format

### Best practices

- Always match the correct dimensions for the target platform (Instagram 1080x1080, LinkedIn 1200x627, etc.)
- Use brand colors consistently across all designs in a squad
- Keep text minimal and high-contrast for readability
- Export at the highest quality available for the target use

### Error handling

- If template search returns no results: broaden the search terms
- If export fails: check the design is complete and all elements are properly placed
- If API rate limit hit: wait 60 seconds before retrying

## Available operations

- **Search Templates** — Find Canva templates by keyword, format, or style
- **Create Design** — Generate a new design from a template or blank canvas
- **Edit Design** — Modify text, images, colors, and layout of an existing design
- **Export Design** — Export a design to PNG, JPG, PDF, or MP4
