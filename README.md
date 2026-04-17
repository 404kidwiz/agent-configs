# agent-configs

> Multi-agent instruction files for a consistent coding experience across Claude Code, Gemini CLI, OpenAI Codex (with oh-my-codex), and Google Antigravity IDE.

## What's in here

| File | Purpose | Target Location | Used By |
|------|---------|-----------------|---------|
| `AGENTS.md` | Universal behavioral spine — principles for every AI coding agent | `~/AGENTS.md` | All tools |
| `CLAUDE.md` | Personal layer, sub-agents, MCP routing, memory system, skills | `~/.claude/CLAUDE.md` | Claude Code CLI |
| `GEMINI.md` | Personal layer, Gemini-specific tool mappings, skill activation | `~/GEMINI.md` | Gemini CLI |
| `codex-AGENTS.md` | Personal layer prepended before oh-my-codex orchestration | `~/.codex/AGENTS.md` | OpenAI Codex CLI |
| `antigravity-instructions.md` | Personal layer, CLI reference, IDE-specific skills | `~/.antigravity/instructions/instructions.md` | Google Antigravity IDE |

## Architecture

```
                    ┌─────────────┐
                    │  AGENTS.md  │  ← Universal behavioral spine
                    │  (shared)   │
                    └──────┬──────┘
                           │ imported by
           ┌───────────────┼───────────────┐
           │               │               │
    ┌──────┴──────┐ ┌──────┴──────┐ ┌──────┴──────────┐
    │  CLAUDE.md  │ │  GEMINI.md  │ │ codex-AGENTS.md  │
    │             │ │             │ │                  │
    │ Sub-agents  │ │ Skills      │ │ Personal layer   │
    │ MCP routing │ │ activate_   │ │ + OMX multi-agent │
    │ Memory sys  │ │ Multi-model │ │ orchestration    │
    │ Skills      │ │ MCP servers │ │ Model routing    │
    └─────────────┘ └─────────────┘ └──────────────────┘
           │
    ┌──────┴──────────────┐
    │ antigravity-        │
    │ instructions.md     │
    │                     │
    │ CLI reference       │
    │ IDE-specific skills │
    └─────────────────────┘
```

### The split

- **AGENTS.md** — tool-agnostic behavioral principles (think before coding, simplicity first, surgical changes, goal-driven execution, ULTRATHINK protocol, design philosophy). Single source of truth for *how to work*.
- **Tool-specific files** — add the personal layer (identity, preferences, stack) and tool-specific mappings. Each file is self-contained and can be used independently.
- **Codex** — uses `~/.codex/AGENTS.md` which prepends personal preferences before the oh-my-codex (OMX) multi-agent orchestration system.

### State storage

| Store | Purpose | Location |
|-------|---------|----------|
| Memory (`MEMORY.md`) | Cross-session recall — identity, feedback, references | `~/.claude/projects/<slug>/memory/` |
| Project summary (`PROJECT_SUMMARY.md`) | Current project state — tasks, progress, blockers | Project root directory |
| Obsidian vault | Long-form knowledge base, task queue, daily notes | `~/Documents/Obsidian Vault/` |

## Verified tool config paths

These paths were confirmed by reading each tool's source code or official documentation:

| Tool | Reads from | Format |
|------|-----------|--------|
| Claude Code | `~/.claude/CLAUDE.md` (global) + `CLAUDE.md` (project) | Markdown, supports `@/path` imports |
| Gemini CLI | `GEMINI.md` — traverses CWD upward to home dir | Plain Markdown, no `@` imports |
| Codex CLI | `~/.codex/AGENTS.md` (global) + `AGENTS.md` (project root → CWD) | Plain Markdown, supports `@import` |
| Antigravity IDE | `~/.antigravity/instructions/instructions.md` (global) + `.github/instructions/*.instructions.md` (project) | Plain Markdown, VS Code-compatible |

## The 4 Principles (from AGENTS.md)

Inspired by [Andrej Karpathy's observations on LLM coding pitfalls](https://x.com/karpathy/status/2015883857489522876), via [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills):

1. **Think Before Coding** — Don't assume. Don't hide confusion. Surface tradeoffs.
2. **Simplicity First** — Minimum code that solves the problem. Nothing speculative.
3. **Surgical Changes** — Touch only what you must. Clean up only your own mess.
4. **Goal-Driven Execution** — Define success criteria. Loop until verified.

Additional principles:
5. **Library & Stack Discipline** — Use what the project uses. Don't reinvent.
6. **ULTRATHINK Protocol** — When triggered, suspend brevity for maximum-depth analysis.
7. **Intentional Minimalism** — Anti-generic design. Every element earns its place.
8. **Skills & Sub-Agent Delegation** — Scan for applicable skills before acting. Delegate when it fits.

## Installation

### Claude Code

```bash
cp CLAUDE.md ~/.claude/CLAUDE.md
cp AGENTS.md ~/AGENTS.md
```

### Gemini CLI

```bash
# Gemini reads GEMINI.md traversing from CWD up to home directory
cp GEMINI.md ~/GEMINI.md
cp AGENTS.md ~/AGENTS.md
```

### OpenAI Codex

```bash
# Codex reads ~/.codex/AGENTS.md natively (not CODEX.md)
# This file includes personal preferences + oh-my-codex orchestration
cp codex-AGENTS.md ~/.codex/AGENTS.md
cp AGENTS.md ~/AGENTS.md
```

### Google Antigravity IDE

```bash
# Antigravity reads global instructions from this path (confirmed from source)
mkdir -p ~/.antigravity/instructions
cp antigravity-instructions.md ~/.antigravity/instructions/instructions.md
```

### Verify each tool

```bash
gemini -p "What are my coding preferences?"
codex "What are my coding preferences?"
# Antigravity: open a project and ask in the AI chat
```

## Stack Summary

- **Languages:** TypeScript, Python
- **Frontend:** React / Next.js, Tailwind v4, GSAP 3, Framer Motion, Three.js / R3F, shadcn/ui + Radix
- **Backend:** tRPC, Drizzle, Convex, Hono
- **Data:** Postgres (Neon / Supabase), Turso, Cloudflare D1
- **Runtimes:** Vercel, Cloudflare Workers/Pages, Deno Deploy
- **Package managers:** pnpm (JS/TS), uv (Python)
- **AI tools:** Claude Code, Codex (OMX), Gemini CLI, Antigravity

## License

MIT
