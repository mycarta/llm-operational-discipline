# Beyond the Vibes: A Rigorous Guide to AI Coding Assistants and Agents

**Author:** Robert Hafner (Distinguished Engineer, Chicago)
**Published:** 2 March 2026
**URL:** [https://blog.tedivm.com/guides/2026/03/beyond-the-vibes-coding-assistants-and-agents/](https://blog.tedivm.com/guides/2026/03/beyond-the-vibes-coding-assistants-and-agents/)

---

## Summary

Comprehensive guide covering AI coding assistants and agents from a software engineering perspective. Covers the same territory as this repo — context management, scope protection, anti-sycophancy — but grounded in production coding workflows rather than research/writing projects.

---

## Key Concepts Relevant to This Repo

### AGENTS.md as Living Document

Project-level instruction files that persist across sessions. Same function as `Claude_Project_Instructions.md` in this repo but scoped to code repositories. The AGENTS.md standard formalizes what we've been doing with project instructions — giving the model a persistent contract for how to behave in a specific project.

### Spec-Driven Development (OpenSpec)

Design before code. The proposal acts as a contract between human and AI. This prevents the "task-oriented at the expense of the big picture" failure mode — the model executes the immediate request without considering whether it fits the larger architecture. Requiring a spec forces the model to articulate its plan before acting.

### Anti-Sycophancy Through Structure

Tests, linting, and CI as deterministic guardrails that catch what the model won't challenge. When the model agrees with everything the user says (the "Waylon Smithers problem"), structural checks provide an independent verification layer. Directly addresses the sycophancy failure mode documented in this repo's playbook.

### Context Window Management

Explicit discussion of summarization/compaction causing drift. Same failure mode as the "compaction data corruption" documented here — when the model summarizes its own context, specific details get mangled or lost, and the model proceeds confidently on corrupted information.

### Security: The Lethal Trifecta

Simon Willison's framework for AI security risk: private data + external communication + untrusted content. When all three are present, the attack surface is maximized. Relevant for any project where the AI agent has access to sensitive files and can make network requests.

---

## Connection to Bullshit-Detector Project

The skill-file-as-contract pattern — where the agent must state which rules it's applying before acting — is an anti-sycophancy mechanism that emerged from the BS detector architecture. Same principle as Hafner's AGENTS.md but applied to research auditing: the model can't silently skip rules if it has to declare them upfront.
