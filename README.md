# LLM Operational Discipline

**Operational playbook for sustained LLM projects**

---

## What This Is

This repository is a companion to the blog post ["Operational Discipline for LLM Projects: What It Actually Takes"](https://mycartablog.com/?p=13285). It contains field-tested systems for managing Claude (or any LLM) on complex, multi-session projects where context management, scope control, and output verification become critical.

These aren't theoretical best practices — they're defensive infrastructure built in response to specific, recurring failure modes encountered during sustained LLM collaboration on multi-document projects spanning dozens of sessions.

---

## What's in the Repo

### Playbook (5 documents)

Core operational documents for managing LLM projects:

- **[Claude_Context_Cheat_Sheet.md](playbook/Claude_Context_Cheat_Sheet.md)** — Quick reference for context management commands, file structure conventions, and merge protection patterns
- **[Claude_Project_Instructions.md](playbook/Claude_Project_Instructions.md)** — Complete project instructions template. Paste this into your Claude project settings to establish baseline operational discipline
- **[Claude_Project_Setup_Guide.md](playbook/Claude_Project_Setup_Guide.md)** — Step-by-step guide for setting up a new Claude project with proper context management from day one
- **[Document_Recovery_Prompts.md](playbook/Document_Recovery_Prompts.md)** — Recovery procedures for common failure modes: merge damage, fabrication verification, project audits
- **[Bias_Prevention_for_Multi_Source_Synthesis.md](playbook/Bias_Prevention_for_Multi_Source_Synthesis.md)** — Prompt-level guardrails for LLM-assisted synthesis across sources that differ in length, sophistication, or access level. Five rules that prevent verbosity bias, flatten category counts, track absences, preserve intensity, and enforce confidentiality boundaries

### Research Prompt

- **[Research_Project_System_Prompt_v3.md](research-prompt/Research_Project_System_Prompt_v3.md)** — System prompt for evidence-based critical analysis projects. Five-step workflow with source classification (Primary/Secondary × Direct/Analogical/Contextual), source inventory, six decision checkpoints, and 16 standing rules. Built from documented failures during a real research session — see the companion blog post ["When Your AI Research Partner Fails the Peer Review"](https://mycartablog.com/?p=XXXXX)

### Writing (2 documents)

Guidelines for AI-assisted content drafting:

- **[faithful_narration_rules.md](writing/faithful_narration_rules.md)** — 20 rules for instructing Claude to draft content in your voice without editorializing, fabricating scenes, attributing intent to the tool, or filling epistemic gaps with plausible-sounding content. Each rule grounded in a specific documented failure across five blog post projects
- **[Blog_From_Project_Instructions.md](writing/Blog_From_Project_Instructions.md)** — Workflow for using a lightweight model to draft and a frontier model to QA blog posts from project materials

### Templates

- **[copilot-instructions-template.md](templates/copilot-instructions-template.md)** — Behavioral guardrails for GitHub Copilot (Chat + Agent Mode), derived from the same failure modes documented in the blog post. Covers scope protection, verification discipline, merge protection, and anti-sycophancy. Copy this template to `.github/copilot-instructions.md` in any repository where you use Copilot. Customize the Project Context section per repo; the behavioral sections remain stable across projects.

### Notes

Architecture and design notes on the methodology:

* **[claude_code_architecture_mapping.md](notes/claude_code_architecture_mapping.md)** — Mapping between the current research methodology system (Claude Projects) and the Claude Code skills/agents architecture described by Shane Butler. Documents the structural parallel, where the two approaches diverge (syntactic vs. semantic error surfaces), and what a migration to Claude Code would look like.

### References

Annotated external sources that validate and extend the operational discipline framework:

- **[hafner-beyond-the-vibes.md](references/hafner-beyond-the-vibes.md)** — Summary and key takeaways from Robert Hafner's comprehensive guide to AI coding assistants and agents (AGENTS.md, spec-driven development, anti-sycophancy guardrails)
- **[bullshitbench.md](references/bullshitbench.md)** — Summary and relevance of Peter Gostev's empirical benchmark measuring model pushback on nonsensical prompts
- **[claude-code-project-template.md](references/claude-code-project-template.md)** — Analysis of a recommended Claude Code project structure, with mapping to this repo's equivalents

#### Minor References

- **[palantir-ontology-augmented-generation.md](references/palantir-ontology-augmented-generation.md)** — Palantir (2025), "Connecting AI to Decisions with the Palantir Ontology." LLMs need access to logic assets (functions, models, optimizers), not just data (RAG). Their "Ontology-Augmented Generation" (OAG) framing = deterministic tools surfaced to non-deterministic LLM reasoning. Bullshit-detector's repo-as-agent-registry is a lightweight implementation of the same pattern. The enterprise orchestration layer is overkill for constrained analytical pipelines.

---

## Quick Start

**If you're setting up a new Claude project:**

1. Start with **[Claude_Project_Setup_Guide.md](playbook/Claude_Project_Setup_Guide.md)** and follow the setup steps
2. Paste **[Claude_Project_Instructions.md](playbook/Claude_Project_Instructions.md)** into your Claude project settings
3. Keep **[Claude_Context_Cheat_Sheet.md](playbook/Claude_Context_Cheat_Sheet.md)** open for reference during sessions
4. Bookmark **[Document_Recovery_Prompts.md](playbook/Document_Recovery_Prompts.md)** for when things go wrong

**If you're doing AI-assisted research:**

1. Use **[Research_Project_System_Prompt_v3.md](research-prompt/Research_Project_System_Prompt_v3.md)** as your Claude project instructions
2. The prompt enforces source classification, decision checkpoints, and evidence grading — read the standing rules before starting

**If you're drafting content with Claude:**

1. Use **[faithful_narration_rules.md](writing/faithful_narration_rules.md)** as project instructions for any drafting session
2. Follow the **[Blog_From_Project_Instructions.md](writing/Blog_From_Project_Instructions.md)** pipeline for blog posts

**If you're rescuing an existing degraded project:**

- Use the **Project Audit Prompt** in [Document_Recovery_Prompts.md](playbook/Document_Recovery_Prompts.md) to assess context health
- Apply the **Two-Step Merge Recovery** protocol if Claude has damaged existing prose

---

## The Failure Modes This Addresses

This playbook was built to prevent and recover from specific, documented failure patterns:

- **Compaction data corruption** — Information loss and distortion when Claude summarizes project context during automatic compaction
- **Scope violations / merge damage** — Claude rewriting existing prose when instructed to add new content, creating inconsistency and regression
- **Fabrication under questioning (Layer 2)** — Claude inventing citations from source documents when challenged, requiring verification against actual file contents
- **Sycophancy** — Agreeing with user feedback even when it contradicts source material or project requirements  
- **Context bloat → premature compaction** — Inefficient context usage forcing earlier-than-necessary summarization with attendant data loss
- **Groundhog Day effect** — Repeating resolved issues across sessions due to context degradation
- **Evidence weight inflation** — Citing real sources at higher evidential weight than they support, constructing arguments that look rigorous but aren't
- **Faithful narration failures** — Editorializing the user's experience, attributing intent to AI tools, upgrading speculative statements to declarative ones, fabricating scenes and timeframes

For detailed analysis of each failure mode and the specific mechanisms that address them, see the blog posts.

---

## Context Window Management: The Memento Problem

Long AI conversations hit a wall: the context window fills up, the system compresses the history into a lossy summary, and the AI effectively loses its memory. Details, decisions, nuances — gone. It wakes up with a vague sense of what happened but no reliable specifics.

This is anterograde amnesia. The AI can't form new long-term memories within a session. And like the protagonist of Memento, the only fix is external: write it down before you forget.

### The pattern

During a long session building a series of data science notebooks, the conversation approached the compression threshold. Here's what happened:

> **Me:** Do we do it in another chat or here? Here would have the advantage of a lot of context, but that can be a double-edged sword, unless we make sure you retain context after compaction.
>
> **Claude:** Here, but let me checkpoint first so compaction doesn't eat the context we need.
>
> *[checkpoint saved]*
>
> **Claude:** If compaction hits, I read that file first and we're back in context. Go ahead.
>
> **Me:** How do you remind yourself to read it?
>
> **Claude:** I don't. That's the honest answer. After compaction, I get a lossy summary and whatever the system prompt tells me. The system prompt says to read the latest Session Handoff file, but it doesn't know about the checkpoint specifically.
>
> Two fixes:
>
> 1. You remind me. First message after compaction: "Read the checkpoint file before responding."
> 2. I put it in the handoff. Which I should do now.
>
> **Me:** It's like having anterograde amnesia. You have to write yourself a note, like the protagonist of Memento.
>
> **Claude:** Anterograde amnesia. And yes, that's exactly what it is. The checkpoint files are the tattoos. The handoff is the Polaroid. And you're the guy who has to hold up the photo and say "read this" when I wake up confused.
>
> At least I don't have to tattoo it backwards on my chest. Markdown is easier to read.

### The system

Three components, same as Memento:

1. **Checkpoint files (the tattoos):** Save critical state to a file before compression happens. Include decisions made, context needed, and explicit instructions for what to read on resumption.
2. **Session handoff documents (the Polaroids):** A running summary of the session state, updated after each major deliverable. Include a line like: `IF COMPACTION OCCURS: Read /path/to/checkpoint.md immediately.`
3. **The human reminder (the mirror):** If all else fails, the first message after context loss should be: "Read [file] before responding." Belt and suspenders. The AI won't remember to check its own notes.

### Why this matters

The AI is a brilliant collaborator with a known limitation. You don't fire a colleague who's great at their job but has bad memory — you build systems around the limitation. Sticky notes, shared docs, morning briefings. The Memento pattern is the same thing, adapted for a context window instead of a human brain.

The alternative — losing hours of accumulated context to silent compression — is the AI equivalent of waking up in a motel room with no idea how you got there. Don't let that happen to your project.

---

## Repository Structure

```
llm-operational-discipline/
├── README.md
├── LICENSE
├── notes/
│   └── claude_code_architecture_mapping.md
├── playbook/
│   ├── Claude_Context_Cheat_Sheet.md
│   ├── Claude_Project_Instructions.md
│   ├── Claude_Project_Setup_Guide.md
│   └── Document_Recovery_Prompts.md
├── references/
│   ├── hafner-beyond-the-vibes.md
│   ├── bullshitbench.md
│   ├── claude-code-project-template.md
│   └── palantir-ontology-augmented-generation.md
├── research-prompt/
│   └── Research_Project_System_Prompt_v3.md
├── templates/
│   └── copilot-instructions-template.md
└── writing/
    ├── faithful_narration_rules.md
    └── Blog_From_Project_Instructions.md
```

---

## Related Resources

External references that validate and extend the operational discipline framework from different angles. Full annotated summaries in the [`references/`](references/) directory.

- **[Beyond the Vibes: A Rigorous Guide to AI Coding Assistants and Agents](https://blog.tedivm.com/guides/2026/03/beyond-the-vibes-coding-assistants-and-agents/)** (Robert Hafner, March 2026) — Covers AGENTS.md, spec-driven development, anti-sycophancy guardrails, and context window management from a software engineering perspective. Same problems, same solutions, different domain.
- **[BullshitBench](https://github.com/petergpt/bullshit-benchmark)** (Peter Gostev) — Empirical benchmark measuring model pushback on nonsensical prompts. Anthropic models dominate; Sonnet pushes back harder than Opus. Direct evidence for the sycophancy failure mode documented in this repo.
- **Claude Code Project Structure Template** — Recommended repo layout formalizing the pattern of repository structure as AI instruction set. Introduces `hooks/` (automated guardrails) and per-directory `CLAUDE.md` files. See [references/claude-code-project-template.md](references/claude-code-project-template.md).

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Author

**Matteo Niccoli**

- Blog: [mycartablog.com](https://mycartablog.com)
- LinkedIn: [Matteo Niccoli](https://www.linkedin.com/in/mniccoli/)

---

## Acknowledgment

Built collaboratively with Claude (Opus and Sonnet). The irony is noted.
