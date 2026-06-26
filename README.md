# zhan-skills

[![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-D97757?logo=anthropic&logoColor=white)](https://docs.claude.com/en/docs/claude-code/skills)
[![Lang](https://img.shields.io/badge/lang-EN%20%2B%20繁中-blue)](#)
[![Skills](https://img.shields.io/badge/skills-1-green)](#skills)
[![Last commit](https://img.shields.io/github/last-commit/zhankai3121/zhan-skills)](https://github.com/zhankai3121/zhan-skills/commits/main)
[![Repo size](https://img.shields.io/github/repo-size/zhankai3121/zhan-skills)](https://github.com/zhankai3121/zhan-skills)

Claude Code / AI agent skills. / 給 Claude Code 與 AI agent 的 skill 集。

## Skills

### `agent-foundations`
Foundational discipline for working as a coding agent **plus** a method for
designing and maintaining an agent memory system.

- **Part A — Coding discipline / 編碼紀律:** think before coding, simplicity
  first, surgical changes, goal-driven execution.
- **Part B — Memory system / 記憶系統:** a 3-tier on-demand architecture
  (L0 index / L1 working / L2 topic) + five design principles to keep base
  context under ~20% of the window.

Detail in [`agent-foundations/SKILL.md`](agent-foundations/SKILL.md), with full
guide and prompt library under `agent-foundations/references/`.

## Install

Copy a skill folder into your skills directory:

```bash
# user-level (all projects)
cp -r agent-foundations ~/.claude/skills/

# or project-level
cp -r agent-foundations .claude/skills/
```

The skill auto-triggers on its description (coding work, editing memory/config
files, or setting up / optimizing an agent's memory). Invoke explicitly with
`/agent-foundations` if your client supports it.

## Layout

```
zhan-skills/
├── README.md
└── agent-foundations/
    ├── SKILL.md
    └── references/
        ├── memory-system.md     # full 3-tier design guide
        └── prompt-library.md    # copy-paste build/reorg prompts
```

## Usage examples / 使用範例

The skill auto-triggers from natural language — no need to name it. / 自然語句即觸發，不必點名。

**Part A — coding discipline / 編碼紀律**

```text
> Refactor this auth module.
  → applies surgical-changes + simplicity: touches only what's asked,
    flags unrelated dead code instead of deleting it.

> Fix the failing login bug.
  → goal-driven: writes a reproducing test first, then makes it pass.

> Add a config option for retry count.
  → think-first: asks before adding speculative flexibility nobody needs.
```

**Part B — memory system / 記憶系統**

```text
> My agent keeps forgetting context / 我的 agent 一直失憶。
  → diagnoses bloated MEMORY.md, proposes 3-tier L0/L1/L2 split.

> Set up a memory system for my project.
  → scaffolds memory/ dir (MEMORY.md, brain.md, user/, topics/, archive/).

> Review my CLAUDE.md / memory files for bloat.
  → audits against the five principles (≤200 lines, SSoT, stable-vs-fluid…).
```

**Explicit invocation / 明確呼叫**

```text
/agent-foundations         # if your client supports slash-skills
```

The reorganize / from-scratch / `brain.md`-template prompts live in
[`agent-foundations/references/prompt-library.md`](agent-foundations/references/prompt-library.md)
— copy, fill the `[...]` blanks, paste.

## Credits / 出處

- Part A adapted from the behavioral `CLAUDE.md` in
  [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).
- Part B adapted from an AI agent memory-system design guide (2026).
