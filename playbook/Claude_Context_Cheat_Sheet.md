# How Claude's Context Works — Note to Self

_February 2026_

Every chat starts by loading all project files + system instructions into a fixed 200K token window. That's the same on Pro ($20) and Max ($200) — the 1M window is API-only.

When the window fills, Claude **compacts**: summarizes everything lossy, saves the full transcript to `/mnt/transcripts/`. The summary becomes what Claude "remembers." **The summary can corrupt details** — I caught it mangling prices and rules from Casa Monteripido. Always ask Claude to verify against transcripts or source files after compaction happens.

**What burns through context fastest:** project files loaded at startup (biggest factor), web searches (each one dumps pages of text), and Opus's long responses/thinking. Sonnet buys ~20–30% more runway through shorter outputs but same window size.

**What I can control:**
- Keep project files lean — every file loads into every chat
- Don't let Claude do 5+ web searches in a single turn
- Break sessions at natural checkpoints rather than pushing through multiple compactions
- Use Sonnet for research-heavy sessions, Opus when I need depth

**Scale reference:** 200K tokens ≈ 150K words ≈ 300 pages of prose. Sounds like a lot until 10 project files eat 40–80K before I type a word.

---

## When to Start a New Chat

There's no formula, but there are natural break points. The goal is to keep each chat session focused enough that it finishes its job before context degrades.

**Break when the task type changes.** Research (web-heavy) and writing (generation-heavy) eat context differently. A session that does both hits the wall fast. On 11 Feb 2026, the university project had a single chat session that went from file pruning → email template updates → Anthropic documentation research (5+ web searches) → pricing investigation → API learning guide. Each topic shift added weight while the earlier topics sat uselessly in the window. That session hit double compaction and produced the Casa Monteripido data corruption bug. In hindsight, the file pruning, the email updates, and the API research should have been three separate chats.

**Break when a deliverable is done.** Finished the Perugia section of the City Living Guide? Save it, update the handoff, start a new chat for Siena. Clean boundary — nothing from the Perugia research is needed in working memory for Siena. Same principle applies to any project: finish the draft, finish the analysis, finish the comparison table → checkpoint → new chat.

**Break when quality drops.** After compaction, Claude is working from a lossy summary. If responses start feeling vague, or you catch factual errors, or Claude seems to have "forgotten" something you discussed 20 minutes ago — that's the signal. The context has degraded. A fresh chat that reads the session handoff file gets you back to full fidelity. Don't try to power through.

**Don't break mid-task.** The worst time to start a new chat is halfway through something that needs turn-by-turn continuity — drafting an email with back-and-forth revisions, working through a multi-step comparison, debugging a code issue. Finish the task, save the output, then break.

**Practical patterns from the university project:**
- City Living Guide: **one city per chat.** Each city requires 5–8 web searches (housing, transport, climbing, nature, safety). That's enough to fill a session cleanly without compaction.
- Email drafting/updating: **one batch per chat.** Group related emails together (e.g., all reply drafts for one round of responses). Don't mix email drafting with unrelated research.
- File maintenance (pruning, handoff updates, template updates): **lightweight, combine freely.** These don't generate web searches or long outputs, so they can share a session with one other small task.
- Deep research (API pricing, Anthropic docs, university program catalogs): **dedicated chat.** Research sessions consume enormous context through search results. Give them their own space.

**Rule of thumb:** if you'd take a coffee break there in real life, it's probably a good place to start a new chat.
