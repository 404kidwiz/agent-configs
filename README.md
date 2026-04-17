# agent-configs

> Multi-agent instruction files for a consistent coding experience across Claude Code, Gemini CLI, OpenAI Codex, and Google Antigravity IDE.

## What's in here

| File | Purpose | Used By |
|------|---------|---------|
| `AGENTS.md` | Universal behavioral spine — principles that apply to every AI coding agent | All tools |
| `CLAUDE.md` | Claude Code config — personal layer, tool mappings, MCP routing, memory system, sub-agents | Claude Code CLI |
| `GEMINI.md` | Gemini CLI config — personal layer, Gemini-specific tool mappings, skill activation | Gemini CLI |
| `CODEX.md` | Codex config — personal layer, sandbox modes, approval policies, MCP integration | OpenAI Codex CLI |
| `antigravity-instructions.md` | Antigravity IDE instructions — personal layer, CLI reference, Antigravity-specific skills | Google Antigravity IDE |

## Architecture

```
                    ┌─────────────┐
                    │  AGENTS.md  │  ← Universal behavioral spine
                    │  (shared)   │
                    └──────┬──────┘
                           │ imported by
           ┌───────────────┼───────────────┐
           │               │               │
    ┌──────┴──────┐ ┌──────┴──────┐ ┌──────┴──────┐
    │  CLAUDE.md  │ │  GEMINI.md  │ │  CODEX.md   │
    │             │ │             │ │             │
    │ Sub-agents  │ │ Skills      │ │ Sandbox     │
    │ MCP routing │ │ activate_   │ │ Approval    │
    │ Memory sys  │ │ Multi-model │ │ Multi-model │
    │ Skills      │ │ MCP servers │ │ MCP servers │
    └─────────────┘ └─────────────┘ └─────────────┘
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

- **AGENTS.md** — tool-agnostic behavioral principles (think before coding, simplicity first, surgical changes, goal-driven execution, ULTRATHINK protocol, design philosophy). This is the single source of truth for *how to work*.
- **Tool-specific files** — add the personal layer (identity, preferences, stack) and tool-specific mappings (MCP routing, sub-agents, skill activation, CLI flags). Each file is self-contained and can be used independently.

### State storage

Each tool has its own state mechanism:

| Store | Purpose | Location |
|-------|---------|----------|
| Memory (`MEMORY.md`) | Cross-session recall — identity, feedback, references | `~/.claude/projects/<slug>/memory/` |
| Project summary (`PROJECT_SUMMARY.md`) | Current project state — tasks, progress, blockers | Project root directory |
| Obsidian vault | Long-form knowledge base, task queue, daily notes | `~/Documents/Obsidian Vault/` |

## The 4 Principles (from AGENTS.md)

Inspired by [Andrej Karpathy's observations on LLM coding pitfalls](https://x.com/karpathy/status/2015883857489522876):

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
# Global config (already in place)
cp CLAUDE.md ~/.claude/CLAUDE.md

# Universal spine (imported by CLAUDE.md)
cp AGENTS.md ~/AGENTS.md
```

### Gemini CLI

```bash
# Gemini reads GEMINI.md from home directory or project root
cp GEMINI.md ~/GEMINI.md
cp AGENTS.md ~/AGENTS.md  # imported by GEMINI.md
```

### OpenAI Codex

```bash
# Codex reads AGENTS.md by convention, plus CODEX.md for tool-specific config
cp CODEX.md ~/CODEX.md
cp AGENTS.md ~/AGENTS.md
```

### Google Antigravity IDE

```bash
# Antigravity instructions — place in Antigravity config directory
mkdir -p ~/.antigravity/instructions
cp antigravity-instructions.md ~/.antigravity/instructions/instructions.md
```

## Stack Summary

- **Languages:** TypeScript, Python
- **Frontend:** React / Next.js, Tailwind v4, GSAP 3, Framer Motion, Three.js / R3F, shadcn/ui + Radix
- **Backend:** tRPC, Drizzle, Convex, Hono
- **Data:** Postgres (Neon / Supabase), Turso, Cloudflare D1
- **Runtimes:** Vercel, Cloudflare Workers/Pages, Deno Deploy
- **Package managers:** pnpm (JS/TS), uv (Python)
- **AI tools:** Claude Code, Codex, Gemini CLI, Antigravity

## License

MIT
