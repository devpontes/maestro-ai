---
name: image-ai-generator
description: >
  Generate images using AI models (OpenAI DALL-E, Stability AI, or FAL.ai).
  Creates images from text prompts and saves them to the squad output directory.
type: script
version: "1.0.0"
script:
  path: scripts/generate.py
  runtime: python
  dependencies:
    - openai
    - requests
    - pillow
  invoke: "python {skill_path}/scripts/generate.py --prompt \"{prompt}\" --output \"{output_path}\" --provider {provider}"
env:
  - IMAGE_AI_PROVIDER
  - IMAGE_AI_API_KEY
categories: [design, image, ai, automation]
---

# AI Image Generator

## When to use

Use the AI Image Generator when you need to create images from text descriptions — product mockups, social media graphics, illustrations, backgrounds, or any visual content that can be described in a prompt.

## Instructions

### Supported providers

Set `IMAGE_AI_PROVIDER` in your `.env` file:
- `openai` — OpenAI DALL-E 3 (best quality, higher cost)
- `stability` — Stability AI (good quality, moderate cost)
- `fal` — FAL.ai (fast, lower cost)

### Usage in a pipeline step

```bash
python skills/image-ai-generator/scripts/generate.py \
  --prompt "Professional product photo of a 3D printer on a white background, studio lighting" \
  --output "squads/{squad}/output/images/product-photo.jpg" \
  --provider openai
```

### Prompt engineering best practices

- Be specific about style: "photorealistic", "flat design", "watercolor illustration"
- Include composition details: "centered", "birds eye view", "close-up"
- Specify background: "white background", "transparent background", "blurred bokeh"
- Add lighting: "studio lighting", "natural daylight", "dramatic shadows"
- Mention dimensions implicitly: "square format", "landscape", "portrait"

### Error handling

- If API key is invalid: check `IMAGE_AI_API_KEY` in your `.env` file
- If prompt is rejected (content policy): rephrase to remove potentially sensitive terms
- If image quality is low: add more detail to the prompt or switch to a higher-quality provider

## Available operations

- **Generate Image** — Create a new image from a text prompt
- **Batch Generate** — Generate multiple images from a list of prompts
- **Resize Image** — Resize and compress a generated image for social media
