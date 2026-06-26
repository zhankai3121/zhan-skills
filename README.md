# zhan-skills

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

## Credits / 出處

- Part A adapted from the behavioral `CLAUDE.md` in
  [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills).
- Part B adapted from an AI agent memory-system design guide (2026).
