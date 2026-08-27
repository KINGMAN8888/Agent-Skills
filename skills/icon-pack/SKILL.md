---
name: icon-pack
description: Generate a coordinated icon pack as SVG icons. Use when a user asks for an icon set, app icons, UI icons, or a coordinated set of glyphs for a brand or product. Returns individual SVG files with consistent style.
---

# icon-pack

Generates a coordinated set of icons for a theme and returns them as clean SVGs.

## Invocation

```
icon-pack --theme "<theme>" [--style line|filled|duotone] [--count 24|48] [--out ./icons]
```

## Options

| Option | Description | Default |
|--------|-------------|---------|
| `--theme` | Description of the icon set theme | required |
| `--style` | Visual style: line (outline), filled, or duotone | line |
| `--count` | Number of icons to generate | 24 |
| `--sizes` | Export sizes in pixels | 256 |
| `--out` | Output directory | ./icon-pack |

## Output

```
icon-pack/
├── manifest.csv         # name + tags per icon
├── svg/<name>.svg
└── png/<name>@256.png
```

## Workflow (Manual Execution)

When invoked:

1. **Determine the icon set** — list all needed icons for the theme
   - Example for a developer portfolio: `code`, `terminal`, `database`, `cloud`, `git`, `api`, `rocket`, `star`, `check`, `arrow-right`
2. **Design each icon** as a clean SVG:
   - 24×24 viewBox
   - 2px stroke width (for line style)
   - No fills for line style; solid fills for filled style
   - Consistent corner radius
   - Simple, recognizable shapes
3. **Write SVG files** to the output directory
4. **Update manifest.csv** with name, tags, and description

## SVG Template (line style)

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none"
     stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
  <!-- icon paths here -->
</svg>
```

## Project-Specific Use (alsherief-tech-canvas)

Icons needed for the portfolio:
- Tech skill icons (React, Node.js, TypeScript, Docker, PostgreSQL, etc.)
- Section icons (Projects, Skills, Certifications, Contact)
- Social icons (GitHub, LinkedIn, Email)

Note: Most tech logos are available from Devicons or Simple Icons CDN:
```html
<img src="https://cdn.simpleicons.org/react" alt="React" width="32" height="32" />
```
