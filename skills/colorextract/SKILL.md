---
name: colorextract
description: Extract color palettes from screenshots and images using AI vision. Outputs hex values, usage context, and design token names. Use when auditing or matching a design's color system.
---

# Color Extract

Analyze any screenshot or image and extract a complete color palette — hex values, usage context, and design token names.

## Usage

```bash
# Extract from local screenshot
colorextract extract --image ./screenshot.png

# Extract from image URL
colorextract extract --image https://example.com/screenshot.png

# Output as design token JSON
colorextract extract --image ./screenshot.png --format tokens

# Save result to file
colorextract extract --image ./screenshot.png --output ./colors.json
```

## Workflow (Manual Execution)

When invoked:

1. **Load the image** (local file or URL)
2. **Analyze visually** — identify distinct colors used for:
   - Backgrounds (page, section, card)
   - Text (primary, secondary, muted, headings)
   - Borders and dividers
   - Interactive elements (buttons, links)
   - Accent/highlight colors
   - Brand/primary color
3. **Map to design tokens** — name each color by role
4. **Output a structured palette**:

```json
{
  "colors": [
    { "hex": "#0f172a", "name": "background-base", "usage": "Page background", "frequency": "high" },
    { "hex": "#6366f1", "name": "primary", "usage": "Buttons, links, accents", "frequency": "medium" },
    { "hex": "#8b5cf6", "name": "secondary", "usage": "Gradient, highlights", "frequency": "low" },
    { "hex": "#f8fafc", "name": "text-primary", "usage": "Headings, body text", "frequency": "high" },
    { "hex": "#64748b", "name": "text-muted", "usage": "Captions, subtext", "frequency": "medium" }
  ],
  "palette": {
    "primary": "#6366f1",
    "secondary": "#8b5cf6",
    "background": "#0f172a",
    "surface": "#1e293b",
    "text": "#f8fafc",
    "muted": "#64748b",
    "border": "#334155"
  },
  "cssVariables": "
    --color-primary: #6366f1;
    --color-secondary: #8b5cf6;
    --color-background: #0f172a;
    --color-text: #f8fafc;
    --color-muted: #64748b;
  "
}
```

## Project-Specific Use (alsherief-tech-canvas)

To audit the current portfolio design:
1. Take a screenshot of `http://localhost:8080/en`
2. Run colorextract against it
3. Compare with `frontend/src/index.css` CSS variables
