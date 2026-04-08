# visual-explainer

This project contains the **visual-explainer** agent skill — generates beautiful, self-contained HTML pages for diagrams, architecture overviews, diff reviews, plan reviews, slide decks, and data tables, instead of ASCII art and box-drawing tables.

## Loading the skill

Read these files to activate the full skill:

1. `plugins/visual-explainer/SKILL.md` — workflow, design principles, aesthetic directions, all diagram types
2. `plugins/visual-explainer/references/css-patterns.md` — CSS layouts, depth tiers, animations, overflow protection
3. `plugins/visual-explainer/references/libraries.md` — Mermaid, Chart.js, Google Fonts, ELK layout
4. `plugins/visual-explainer/references/responsive-nav.md` — sticky sidebar TOC for multi-section pages
5. `plugins/visual-explainer/references/slide-patterns.md` — slide deck engine, transitions, presets

Before generating, read the reference template that matches the content type:

| Content | Template |
|---|---|
| Architecture overviews, card layouts | `plugins/visual-explainer/templates/architecture.html` |
| Flowcharts, sequence diagrams, Mermaid | `plugins/visual-explainer/templates/mermaid-flowchart.html` |
| Data tables, comparisons, audits | `plugins/visual-explainer/templates/data-table.html` |
| Slide decks | `plugins/visual-explainer/templates/slide-deck.html` |

## Commands

Read the corresponding command file when the user requests:

| Request | Command file |
|---|---|
| Diagram, flowchart, architecture | `plugins/visual-explainer/commands/generate-web-diagram.md` |
| Implementation plan | `plugins/visual-explainer/commands/generate-visual-plan.md` |
| Slide deck / presentation | `plugins/visual-explainer/commands/generate-slides.md` |
| Diff review / code review | `plugins/visual-explainer/commands/diff-review.md` |
| Plan review | `plugins/visual-explainer/commands/plan-review.md` |
| Project recap / mental model | `plugins/visual-explainer/commands/project-recap.md` |
| Fact-check a document | `plugins/visual-explainer/commands/fact-check.md` |
| Share / deploy HTML | `plugins/visual-explainer/commands/share.md` |

## Delivering output

Detect the environment and adapt accordingly:

**Claude Code, Claude Desktop, Pi, Codex** (file system available):
- Write to `~/.agent/diagrams/<descriptive-name>.html`
- Open: `open ~/.agent/diagrams/<file>` (macOS) or `xdg-open ~/.agent/diagrams/<file>` (Linux)
- Tell the user the file path

**Claude.ai web** (no direct file system):
- Output the complete self-contained HTML as an artifact (if supported) or inside a fenced `html` code block
- Tell the user to save the content as a `.html` file and open it in their browser

## Triggers

Activate automatically when:
- The user asks for a diagram, chart, flowchart, architecture overview, or any visual explanation of a technical concept
- A complex table with 4+ rows or 3+ columns is about to be rendered — generate HTML instead of ASCII
- The user invokes a command: `/diff-review`, `/plan-review`, `/project-recap`, `/fact-check`, `/generate-web-diagram`, `/generate-slides`, `/generate-visual-plan`, `/share`
  - In Claude Code: namespaced as `/visual-explainer:command-name`
  - In Claude.ai: invoke by describing the workflow or using the command name

Never fall back to ASCII art or box-drawing tables when this skill is loaded.
