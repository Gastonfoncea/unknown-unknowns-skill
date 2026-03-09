# Unknown Unknowns Detector

> 🇪🇸 [Leer en español](README.es.md)

A skill for Claude Code (and compatible AI coding tools) that surfaces the risks, gaps, and blind spots you don't know you're missing — before they become problems.

## The Problem

When you're building something, your knowledge falls into three buckets:

- **Known knowns** — Things you know and have handled.
- **Known unknowns** — Things you know you need to figure out.
- **Unknown unknowns** — Things you have no idea you should be thinking about.

The last one is the most dangerous. You can't Google what you don't know exists. This skill cross-references what you're building against deep domain knowledge and surfaces the gaps you can't see from your current vantage point.

It works whether you're a seasoned developer missing edge cases outside your domain, or a vibecoder building your first real app.

## What It Does

1. **Detects your mode** — Do you have a codebase, or just an idea? If there's code, it scans your project first to find blind spots automatically. If it's an idea, it asks targeted questions.

2. **Scans your codebase (if available)** — Reads your project structure, dependencies, config, networking layer, and data models. Finds issues without you having to describe them.

3. **Asks only what it can't infer** — No generic questionnaires. Questions are specific to what you're building and what stage you're at.

4. **Analyzes across the dimensions you choose:**
   - Security & vulnerabilities
   - Architecture & scalability
   - Legal / compliance / regulatory
   - Costs & operations
   - Product & business blind spots
   - AI-specific risks (agents, LLMs)
   - UX & accessibility
   - Data management & privacy

5. **Delivers prioritized findings:**
   - 🔴 **Critical** — Could kill your project or cause serious harm
   - 🟡 **Important** — Will cause pain if ignored
   - 🟢 **Worth knowing** — For when you scale

6. **Ends with an actionable top-5 list** — Not a repeat of findings, but "if you do nothing else, do these things."

## Installation

# Global (todos los proyectos)
cp -r unknown-unknowns-skill ~/.claude/skills/unknown-unknowns

# O para un proyecto específico
cp -r unknown-unknowns-skill .claude/skills/unknown-unknowns

## Usage

The skill triggers automatically when you say things like:

- "What am I missing in this project?"
- "Review my architecture for blind spots"
- "What are the unknown unknowns here?"
- "Audit my project before launch"
- "What could go wrong?"

You can also be direct:

- "Run the unknown unknowns skill on this project"
- "Analyze the security and architecture blind spots"

## Example

Here's a condensed example analyzing a macOS note-taking app with AI integration:

**Discovery:**
> "What are you building, what stage, and which dimensions should I analyze?"

**User:** "A macOS app in Swift that captures quick notes via keyboard shortcut and uses OpenAI to classify them. Pre-launch, freemium model. Analyze architecture."

**Analysis output (abbreviated):**

> 🔴 **Critical: The API key lives in the client**
>
> Your app connects directly to OpenAI — the key has to be somewhere on the user's machine. Anyone can extract it from the binary and use it at your cost. You need a backend proxy.
>
> 🟡 **Important: No abstraction over the AI provider**
>
> OpenAI calls scattered across the codebase means adding Claude or a local model later requires rewriting half the app. Create an `AIProvider` protocol now.
>
> 🟢 **Worth knowing: Auto-update mechanism**
>
> Distributing outside the App Store means you need Sparkle or similar for updates.
>
> **Top actions:** (1) Decide if you need a backend — your entire business model depends on it. (2) Create the AI provider abstraction. (3) Add retry + timeout on API calls.

## How It Works

```
unknown-unknowns/
├── SKILL.md              # Core skill logic and workflow
└── references/
    ├── security.md       # Security blind spots checklist
    ├── architecture.md   # Architecture blind spots checklist
    ├── legal.md          # Legal/compliance checklist (includes LATAM)
    ├── costs.md          # Cost & operations checklist
    ├── product.md        # Product & business blind spots checklist
    └── ai-agents.md      # AI agent-specific risks checklist
```

The skill reads only the reference files relevant to the dimensions you chose — it doesn't load everything into context.

## Compatibility

Built for Claude Code, but the SKILL.md format works with any tool that supports it:

- Claude Code
- Cursor
- OpenCode
- Windsurf
- Cline
- Roo Code

## Contributing

Found a blind spot the skill should catch? Open an issue or PR. The reference files are designed to grow — adding new items to a checklist is the easiest way to contribute.

## License

MIT
