---
name: deepresearch
description: Perform deep research on any topic using web search and AI synthesis. Use when you need comprehensive research reports with citations, analysis of topics from multiple angles, or thorough investigation of technical subjects.
---

# Deep Research

Agentic deep research skill using web search for discovery and AI synthesis for comprehensive reporting.

## Invocation

```
/deepresearch <topic>
```

## Options

| Option | Description | Default |
|--------|-------------|---------|
| `--depth <level>` | Research depth: quick (6 queries), normal (15), deep (30) | normal |
| `--output <path>` | Custom output path | ./research/ |
| `--json` | Also save sources as JSON | false |

## Examples

```
# Quick overview of a topic
/deepresearch "What is RAG?" --depth quick

# Standard research
/deepresearch "Best practices for building production React apps"

# Deep research with custom output
/deepresearch "Compare Next.js vs Vite" --depth deep --output ./research.md
```

## Workflow (Manual Execution)

When invoked, follow this process:

1. **Decompose the topic** into 5–15 sub-questions covering different angles
2. **Search in parallel** for each sub-question using web search tools
3. **Scrape and read** the top results for each query
4. **Synthesize findings** into a structured report:
   - Executive summary
   - Key findings (with citations)
   - Detailed analysis per angle
   - Comparison tables where appropriate
   - Conclusions and recommendations
5. **Save the report** as a Markdown file with timestamp

## Output Format

```markdown
# Research Report: <topic>

**Date**: YYYY-MM-DD
**Depth**: normal (15 queries)

## Executive Summary
...

## Key Findings
1. Finding with [source](url)
2. ...

## Detailed Analysis
### Angle 1
...

## Conclusions
...

## Sources
- [Title](url) — brief description
```
