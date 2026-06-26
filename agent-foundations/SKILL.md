---
name: agent-foundations
description: >-
  Foundational discipline for working as a coding agent, plus a method for
  designing and maintaining an AI agent memory system. Part A — coding conduct:
  think before coding, simplicity first, surgical changes, goal-driven
  execution. Part B — a 3-tier on-demand memory architecture (L0 index / L1
  working / L2 topic) with five design principles to keep base context under
  ~20% of the window. Use when starting non-trivial coding work, when editing
  memory/config files (CLAUDE.md, MEMORY.md, brain.md, preferences), or when
  setting up / auditing / optimizing an agent's memory so it stops "forgetting".
  (中文觸發詞：記憶系統、三層架構、context 爆掉、agent 失憶、編碼紀律、精簡改動)
---

# Agent Foundations / Agent 基礎守則

Two parts, both load-bearing. Part A governs **how you change code**. Part B
governs **how the agent remembers**. They share one spine: minimum footprint,
single source of truth, distill often.

> 兩部分：A 管「怎麼改碼」，B 管「agent 怎麼記」。共同脊椎：最小佔用、單一真相、勤蒸餾。

---

## Part A — Coding Discipline / 編碼紀律

Bias toward caution over speed. For trivial tasks, use judgment.
(偏謹慎勝過快；瑣事自行判斷。)

### 1. Think before coding / 先想再寫
Don't assume. Don't hide confusion. Surface tradeoffs.
- State assumptions explicitly. If uncertain, ask.
- Multiple interpretations exist → present them, don't pick silently.
- A simpler approach exists → say so. Push back when warranted.
- Something unclear → stop, name what's confusing, ask.

### 2. Simplicity first / 精簡優先
Minimum code that solves the problem. Nothing speculative.
- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility"/"configurability" nobody requested.
- No error handling for impossible scenarios.
- 200 lines that could be 50 → rewrite it.

Test: would a senior engineer call this overcomplicated? If yes, simplify.

### 3. Surgical changes / 外科式改動
Touch only what you must. Clean up only your own mess.
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor what isn't broken. Match existing style.
- Notice unrelated dead code → mention it, don't delete it.
- Your change orphaned an import/var/fn → remove it. Pre-existing dead code stays unless asked.

Test: every changed line traces directly to the request.

### 4. Goal-driven execution / 目標驅動
Define success criteria. Loop until verified.
- "Add validation" → write tests for invalid inputs, then make them pass.
- "Fix the bug" → write a test that reproduces it, then make it pass.
- "Refactor X" → ensure tests pass before and after.

For multi-step tasks, state a brief plan with a verify check per step.
Strong criteria let you loop independently; weak criteria ("make it work")
force constant clarification.

**Working if:** fewer junk diffs, fewer overcomplication rewrites, clarifying
questions come *before* implementation not after mistakes.

---

## Part B — Memory System / 記憶系統

### Why agents "forget" / 為何失憶
LLMs have no real memory — each session is a blank page rebuilt from memory
files loaded into the prompt. Cram everything into one 800-line `MEMORY.md` and
loading alone eats >30% of the context window. The model then ignores early
content, forgets safety rules, treats old tasks as new — and gets dumber.

> 根因不是模型笨，是 context window 被塞爆。

### The 3-tier architecture / 三層架構
Make memory **on-demand**, mirroring human memory tiers.

| Tier | File(s) | Loads | Holds |
|------|---------|-------|-------|
| **L0 Index** | `MEMORY.md` (≤60 lines) | every session | *not* content — just "what lives where" |
| **L1 Working** | `brain.md` (~80-120 lines), `TOOLS.md` | every session | current focus, what's blocked, awaiting whom, available tools. Clean daily; move done items to journal/archive |
| **L2 Topic** | `topics/*.md`, `projects/*/context.md` | on demand | topic-split knowledge — read insurance context only when insurance comes up |
| L3 Archive | `archive/` | never auto | history / expired records |

### Five design principles / 五大原則
1. **Position is the index** — correct folder = self-indexing. `projects/health/context.md` *means* the health project. No search machinery needed.
2. **≤200 lines per file** — ~3000-4000 tokens is the processing sweet spot. Past it, externalize and split.
3. **SSoT (single source of truth)** — one fact lives in exactly one place. Duplicates cause confused judgment.
4. **Separate stable vs fluid** — stable (birthday, prefs) → L2 `profile`/`topics`; fluid (today's todos) → `brain.md`. Never mix.
5. **Distill regularly** — weekly review. Distill key insight into core cognition; archive expired content to `archive/`.

> Best practice: first line of every file = a comment saying what it is and when to read it.

### Conflict override / 衝突覆寫
L2 `profile.md` says "prefer 繁中" but `projects/x/context.md` needs English →
the agent goes bilingual-garbled. Fix: explicit override declaration in the
project file: `Project language: English (overrides global preference)`.

### When RAG? / 何時用 RAG
- Tens of `.md` files → **stick with the filesystem** (most precise, full control).
- Hundreds of docs / thousand-page KB (e.g. support bot) → only then add RAG (vector + semantic chunk search).

### Deeper material / 深度資料
- Full design guide, directory layout, and pitfalls: [`references/memory-system.md`](references/memory-system.md)
- Copy-paste prompts to build/reorganize a memory system: [`references/prompt-library.md`](references/prompt-library.md)
