---
name: siteanalyze
description: Analyze any website's design system — detects UI framework, extracts colors, typography, and component patterns. Use when you want to reverse-engineer or audit a site's visual design for consistency or inspiration.
---

# Site Analyze

Analyze any website's design system — detects shadcn/ui, Tailwind, extracts colors, typography, and components.

## Usage

```
siteanalyze <url>
```

## What It Does

- Navigates to the target URL
- Takes a screenshot / reads the page HTML/CSS
- Detects UI framework (shadcn/ui, Tailwind CSS, Bootstrap, etc.)
- Extracts color palette, typography scale, and component patterns
- Outputs a design profile compatible with open-styles format

## Workflow (Manual Execution)

When invoked:

1. **Fetch the page** — read the URL content including CSS and HTML structure
2. **Detect framework** — look for class patterns (`tw-`, Tailwind utilities, `chakra-`, etc.), CSS variable names, font imports
3. **Extract colors** — find CSS custom properties (`:root { --primary: ... }`), Tailwind config, inline styles
4. **Extract typography** — Google Fonts imports, `font-family` declarations, heading/body size scale
5. **Identify components** — nav, hero, cards, buttons, forms, footer — describe their structure
6. **Audit consistency** — spacing scale, border radius, shadow patterns

## Output Format

```json
{
  "url": "https://example.com",
  "framework": "Tailwind CSS + shadcn/ui",
  "colors": {
    "primary": "#6366f1",
    "secondary": "#8b5cf6",
    "background": "#0f172a",
    "text": "#f8fafc",
    "muted": "#64748b"
  },
  "typography": {
    "heading": "Inter, sans-serif",
    "body": "Inter, sans-serif",
    "scale": ["12px", "14px", "16px", "20px", "24px", "32px", "48px"]
  },
  "components": ["Navbar", "Hero", "FeatureGrid", "TestimonialsCarousel", "Footer"]
}
```

## Project-Specific Use (alsherief-tech-canvas)

Run against the local dev server to audit the portfolio design:
```
siteanalyze http://localhost:8080/en
```
