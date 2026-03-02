# Claude Code Project Structure Template

**Source:** LinkedIn post (whiteboard photo — no permanent URL)
**Format:** Recommended repository layout for Claude Code projects

---

## Structure

```
claude_code_project/
├── CLAUDE.md                    # Project memory and instructions
├── README.md
├── docs/
│   ├── architecture.md
│   ├── decisions/
│   └── runbooks/
├── .claude/
│   ├── settings.json
│   ├── hooks/                   # Guardrails and automation checks
│   └── skills/                  # Reusable AI workflows
│       ├── code-review/SKILL.md
│       ├── refactor/SKILL.md
│       └── release/SKILL.md
├── tools/
│   ├── scripts/
│   └── prompts/
└── src/
    ├── api/CLAUDE.md
    └── persistence/CLAUDE.md
```

---

## Why It Matters

This template formalizes the pattern that the bullshit-detector project arrived at organically: **repository structure as AI instruction set.** Each directory can have its own `CLAUDE.md` with context-specific instructions, so the model receives only the rules relevant to the code it's currently touching.

---

## Key Concepts

### Distributed Project Instructions

Instead of one monolithic instruction file, `CLAUDE.md` files are placed at each level of the repo hierarchy. The top-level `CLAUDE.md` holds global rules; subdirectory-level files hold context-specific instructions. This maps directly to how this repo's playbook separates concerns across multiple documents — but applies the principle at the file-system level.

### Skills as Reusable Workflows

The `.claude/skills/` directory contains `SKILL.md` files — named, scoped instruction sets for specific tasks (code review, refactoring, release). This is the same pattern as the skill-file-as-contract in the bullshit-detector architecture: the model loads a specific skill before acting, which bounds its behavior and makes rule application explicit.

### Hooks: Automated Guardrails

The `.claude/hooks/` directory holds automated checks that fire before or after agent actions. This is a concept this repo doesn't yet have. Hooks are **deterministic guardrails** — the same principle as the Document Recovery Prompts in this repo's playbook, but triggered automatically instead of manually invoked by the user.

The hook pattern closes a gap in the current operational discipline framework: right now, all verification is human-initiated (the user has to remember to run the recovery prompt). Hooks make verification structural — it happens whether the user remembers or not.

### Decisions Directory

The `docs/decisions/` directory captures architectural decisions explicitly. Same function as session handoffs in this repo — preserving the reasoning behind choices so future sessions (or future humans) can understand why something was done a particular way, not just what was done.

---

## Mapping to This Repo

| Claude Code Template | This Repo's Equivalent |
|---|---|
| `CLAUDE.md` (top-level) | `Claude_Project_Instructions.md` |
| `CLAUDE.md` (per-directory) | No equivalent yet — instructions are global |
| `.claude/skills/` | Playbook documents (each scoped to a task type) |
| `.claude/hooks/` | `Document_Recovery_Prompts.md` (manual, not automated) |
| `docs/decisions/` | Session handoff files |
| `docs/runbooks/` | `Claude_Project_Setup_Guide.md` |
