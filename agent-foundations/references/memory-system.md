# AI Agent Memory System — Full Design Guide (2026)
# AI Agent 記憶系統設計與優化指南

> **TL;DR** — The root cause of agent amnesia is not weak models; it's a context
> window stuffed full. A 3-tier architecture (L0 index / L1 working / L2 topic)
> plus five principles keeps base footprint under ~15-20% of context, sharply
> improving stability and apparent intelligence.
>
> 失憶根因是 context window 被塞爆。三層架構 + 五原則把基礎佔用壓到 15-20% 以內。

---

## 1. Why agents forget / 為何 AI Agent 會失憶

LLMs (Gemini, Claude, ChatGPT) have no true memory. Every conversation starts
as a blank page; context is held only by the memory files loaded into the prompt
before the conversation begins.

- **Pain point:** stuff every setting (preferences, state, rules, logs) into one
  file — e.g. an 800+ line `MEMORY.md` — and loading alone consumes >30% of the
  context window.
- **Consequence:** the model starts ignoring early content, forgetting safety
  rules, mistaking old tasks for new ones — performance degrades, it acts dumb.

---

## 2. Core architecture: 3-tier memory (L0 / L1 / L2)

Borrowing from human memory categories, make memory **on-demand**.

### L0 — Index layer (Boot / auto-loaded every session)
- **Example file:** `MEMORY.md` (within ~60 lines)
- **Function:** stores no actual content — only tells the agent *what info lives
  where*.
- **Structure example:**

```markdown
## L0 Boot (auto-load)
- SOUL.md: core values
- MIND.md: evolving persona
- AGENTS.md: operating rules

## L1 Session (load each time)
- brain.md: current working memory, shorthand version
- TOOLS.md: local tools and environment

## L2 On Demand (load on demand)
- memory/user/profile.md: basic profile
- projects/*/context.md: project context
```

### L1 — Working memory (Session / loaded each conversation)
- **Example files:** `brain.md` (~80-120 lines, cleaned daily), `TOOLS.md`
- **Function:** holds "what am I doing now", "what's blocked", "waiting on whom",
  and available tool APIs. Completed tasks must be periodically moved out to a
  journal.

### L2 — Topic memory (On demand / loaded when needed)
- **Example files:** `topics/*.md`, `projects/*/context.md`
- **Function:** split info by topic. Read insurance context only when insurance
  comes up; read a project's data only when that project comes up. Everything
  irrelevant stays unloaded.

---

## 3. Five memory design principles / 五大記憶設計原則

1. **Position is the index / 位置就是索引** — a file in the correct folder is
   self-indexing (e.g. `projects/health/context.md` denotes the health project).
   No complex search mechanism needed.
2. **≤200 lines per file / 單檔不超過 200 行** — 200 lines (~3000-4000 tokens) is
   the agent's processing sweet spot. Beyond it, externalize and split into a new
   file.
3. **SSoT (single source of truth) / 單一真相來源** — one fact may exist in only
   one place. Duplicated info confuses the agent's judgment.
4. **Separate stable vs fluid / 區分穩定與流動** — stable (birthday, prefs) →
   L2 `profile` or `topics`; fluid (today's todos) → `brain.md`. Never mix them.
5. **Distill regularly / 定期蒸餾** — weekly memory review. Distill key insights
   into core cognition; archive expired content to `archive/`.

---

## 4. Recommended directory structure / 推薦目錄結構

Deploy this structure straight into a project directory:

```bash
memory/
├── MEMORY.md               # L0: index file (within 60 lines)
├── brain.md                # L1: what I'm doing today (clean & update daily)
├── user/
│   ├── profile.md          # L2: personal profile
│   └── preferences.md      # L2: work/communication preferences
├── topics/
│   ├── topic-a.md          # L2: stable topic knowledge A
│   └── topic-b.md          # L2: stable topic knowledge B
└── archive/                # L3: historical archive & expired records
```

> 💡 **Best practice:** the *first line* of every file should be a comment
> describing what the file is and when it should be read.
> 每個檔案第一行寫註解，說明用途與何時讀。

---

## 5. Common pitfalls & fixes / 常見踩坑與解決方案

**Working memory (`brain.md`) inflates without bound / brain.md 無限膨脹**
- *Problem:* forgetting to clean `brain.md` piles up months-old tasks; the agent
  treats an old bug as a new task and goes off to "fix" it.
- *Fix:* force daily cleanup. Items older than three days, if unfinished, must
  have their status updated or be moved to `journal`/archive. Consider a cron job
  for daily optimization.

**Memory conflict / 記憶衝突**
- *Problem:* L2 `profile.md` says "prefer 繁體中文" but a project's `context.md`
  says "this project uses English" — the agent garbles a Chinese/English mix.
- *Fix:* add an explicit override declaration in the project's `context.md`:
  `Project language: English (overrides global preference)` — tells the agent
  which weight wins.

**When to use RAG (vector DB)? / 何時用 RAG**
- Memory is only dozens of `.md` files → **stick with the filesystem** (most
  precise, full control).
- Memory reaches hundreds of documents or a thousand-page knowledge base (e.g. a
  support bot) → only then introduce RAG (e.g. Dify's built-in mechanism) for
  chunked semantic search.
