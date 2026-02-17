# LLM Operational Discipline

**Operational playbook for sustained LLM projects**

---

## What This Is

This repository is a companion to the blog post ["Operational Discipline for LLM Projects: What It Actually Takes"](https://mycartablog.com/?p=13285&preview=1&_ppp=c082ea2798). It contains field-tested systems for managing Claude (or any LLM) on complex, multi-session projects where context management, scope control, and output verification become critical.

These aren't theoretical best practices — they're defensive infrastructure built in response to specific, recurring failure modes encountered during sustained LLM collaboration on multi-document projects spanning dozens of sessions.

---

## What's in the Repo

### Playbook (4 documents)

Core operational documents for managing LLM projects:

- **[Claude_Context_Cheat_Sheet.md](playbook/Claude_Context_Cheat_Sheet.md)** — Quick reference for context management commands, file structure conventions, and merge protection patterns
- **[Claude_Project_Instructions.md](playbook/Claude_Project_Instructions.md)** — Complete project instructions template. Paste this into your Claude project settings to establish baseline operational discipline
- **[Claude_Project_Setup_Guide.md](playbook/Claude_Project_Setup_Guide.md)** — Step-by-step guide for setting up a new Claude project with proper context management from day one
- **[Document_Recovery_Prompts.md](playbook/Document_Recovery_Prompts.md)** — Recovery procedures for common failure modes: merge damage, fabrication verification, project audits

### Writing

- **[Blog_From_Project_Instructions.md](writing/Blog_From_Project_Instructions.md)** — Instructions for the Sonnet-drafts-Opus-QA blog production pipeline. How to use two models in sequence for structured content production with quality control
- **[faithful_narration_rules.md](writing/faithful_narration_rules.md)** — Rules for faithful narration in writing projects

### Research Prompt

- **[Research_Project_System_Prompt_v3.md](research-prompt/Research_Project_System_Prompt_v3.md)** — System prompt for research projects

---

## Quick Start

**If you're setting up a new Claude project:**

1. Start with **[Claude_Project_Setup_Guide.md](playbook/Claude_Project_Setup_Guide.md)** and follow the setup steps
2. Paste **[Claude_Project_Instructions.md](playbook/Claude_Project_Instructions.md)** into your Claude project settings
3. Keep **[Claude_Context_Cheat_Sheet.md](playbook/Claude_Context_Cheat_Sheet.md)** open for reference during sessions
4. Bookmark **[Document_Recovery_Prompts.md](playbook/Document_Recovery_Prompts.md)** for when things go wrong

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

For detailed analysis of each failure mode and the specific mechanisms that address them, see the full blog post.

---

## Repository Structure

```
llm-operational-discipline/
├── README.md
├── LICENSE
├── playbook/
│   ├── Claude_Context_Cheat_Sheet.md
│   ├── Claude_Project_Instructions.md
│   ├── Claude_Project_Setup_Guide.md
│   └── Document_Recovery_Prompts.md
├── research-prompt/
│   └── Research_Project_System_Prompt_v3.md
└── writing/
    ├── Blog_From_Project_Instructions.md
    └── faithful_narration_rules.md
```

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
