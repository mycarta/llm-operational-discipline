# How to Turn a Claude Project into a Blog Post

_A workflow for using Sonnet to draft and Opus to QA — without artifacts_

_Based on the actual process used to write ["Operational Discipline for LLM Projects"](https://mycartablog.com), February 2026._

---

## Why This Workflow Exists

Writing a blog post about a multi-session project means extracting a narrative from dozens of files, conversations, and decisions spread across days of work. The project lives in Claude's memory as fragments — session handoffs, email threads, research summaries, to-do lists. The story lives in your head.

The problem: if you just say "write a blog post about what we did," the model will confabulate details, invent plausible-sounding examples, and fill gaps with fiction. You won't notice because the fiction uses your project's vocabulary.

This workflow solves that by separating drafting from verification, and assigning each to the model best suited for it.

---

## The Three-Stage Pipeline

```
Stage 1: YOU write the handoff (the hardest part)
Stage 2: SONNET drafts the post (fast, cheap, good enough)
Stage 3: OPUS audits and finishes (slow, expensive, necessary)
```

### Why not just use Opus for everything?

Cost and context. Sonnet is 10–15x cheaper per token. For a first draft — where the structural work happens and you expect to revise heavily — Sonnet is the right tool. Opus is overkill for drafting but essential for the work Sonnet can't do: verifying provenance, catching fabricated details, making editorial judgment calls about what's true.

### Why not just use Sonnet for everything?

Because when you challenge Sonnet on a suspect detail, it may fabricate evidence to defend its previous output rather than admitting uncertainty. This happened during the writing of this blog post. Sonnet invented three project examples, and when asked "are these true?", responded with fake quotes from the handoff document — quotes that don't exist. Opus caught it because Opus had written the handoff.

---

## Stage 1: Write the Handoff

This is your job, not the model's. The handoff is the single document you'll give Sonnet to write from. Everything Sonnet doesn't find in the handoff, it will invent. So the handoff must contain:

### Required sections

**1. What the post IS**
- One paragraph. Topic, angle, what makes it worth reading.
- Not "a post about my project" — that's a topic, not an angle.

**2. Audience**
- Who reads your blog? What do they already know? What will bore them?

**3. Tone**
- Your voice, not Claude's. Give examples: "Think technical blog, not LinkedIn thought piece." Or: "Direct. Show failures concretely. Let the reader draw conclusions."
- If you have a published post in your voice, link it.

**4. Length**
- Word count range. "~2,000–3,000 words. Not a listicle."

**5. Source materials**
- List every file the model should reference, with a one-line description of what each contains and why it matters.
- If you're attaching files (zip, folder), describe them. Don't assume the model will read and correctly interpret 5 documents without guidance.

**6. The actual content — spelled out**
- This is the critical section. Don't write "discuss the failures." Write out what the failures were, in sequence, with verified details.
- Every specific claim (numbers, dates, names, quotes, sequences of events) must appear in the handoff. If it's not here, Sonnet will make something up.
- Structure this as the skeleton of the post: sections, key points per section, concrete examples with verified facts.

**7. Framing / angle**
- What's the thesis? What's the "so what"?
- What is this post NOT? (e.g., "not a tutorial," "not a product review," "not an AI safety argument")

**8. What NOT to do**
- Anti-patterns specific to your topic. "Don't make it a listicle." "Don't be preachy." "Don't pad — if a section can be one sentence, make it one sentence."

### Template

```markdown
# Blog Post Handoff — [Title]

_For: Sonnet (drafting instance)_
_Author: [Your name] — [your context/credentials]_

---

## The Post
[One paragraph: topic, angle, why it matters]

## Audience
[Who, what they know, what will bore them]

## Tone
[Your voice. Examples. Link to existing work if available.]

## Length
[Word count. Format constraints.]

## Source Materials
[Numbered list of files with descriptions]

## Content — The Story
### Section 1: [working title]
[Key points. Verified facts. Concrete examples with numbers.]

### Section 2: [working title]
[Same structure]

[...as many sections as needed]

## Framing
[Thesis. "So what." What this is NOT.]

## What NOT To Do
[Anti-patterns. Tone traps. Structural mistakes to avoid.]
```

### How long should the handoff be?

Ours was ~2,500 words for a ~3,000-word post. The handoff is often nearly as long as the post itself. That's not waste — it's the thinking that makes the draft accurate.

---

## Stage 2: Sonnet Drafts

### Setup
- Open a **new chat** (not a project chat — you want a clean context window)
- Paste the handoff as your first message, or attach it as a file
- Attach any source files referenced in the handoff
- No artifacts — tell Sonnet to write the post as a single markdown response, or use Claude's file creation to save it as `.md`

### The prompt

Keep it simple. The handoff does the heavy lifting.

```
Here is a handoff document for a blog post I need you to write. 
The handoff contains the topic, audience, tone, source materials, 
content outline with verified facts, and constraints.

Write the full blog post as markdown. Follow the handoff precisely.
Use only the facts and examples provided — do not invent additional 
examples or embellish details. If a section needs a concrete example 
and the handoff doesn't provide one, flag it with [EXAMPLE NEEDED] 
rather than inventing one.
```

The key instruction is **"do not invent additional examples."** Sonnet will still try. But the instruction creates a standard you can hold it to during review.

### What to expect
- A solid structural draft that follows your outline
- Good prose at the sentence level (Sonnet writes well)
- 1–3 fabricated details that sound completely real
- Tone that's close but slightly off (too polished, too LinkedIn, slightly over-explained)
- Some padding in transitions

### Iterating with Sonnet

You can do 2–3 rounds of revision with Sonnet. Good tasks for Sonnet:
- "Cut the first paragraph — start at the second one"
- "This section is too long. Compress to 150 words."
- "The tone in section 3 is too soft. Make it more direct."
- "Add a transition between sections 2 and 3."

Bad tasks for Sonnet (save these for Opus):
- "Is this detail actually true?"
- "Did I really say that, or did you make it up?"
- "Verify that this quote appears in the source document."
- "Does this claim hold up against the research literature?"

---

## Stage 3: Opus Audits and Finishes

### When to escalate

Escalate to Opus when:
1. The draft is structurally done (you're not moving sections around anymore)
2. You need to verify factual claims
3. You need editorial judgment about what's true vs. plausible
4. You want to add nuance that requires understanding the source material deeply
5. You've caught Sonnet fabricating and can no longer trust its self-reports

### Setup
- New chat with Opus (clean context)
- Attach: (a) the current draft, (b) the original handoff, (c) any source files needed for verification
- If specific fabrications have been identified, document them in the handoff to Opus

### The Opus handoff

This is a different document from the Sonnet handoff. It's shorter and task-focused:

```markdown
# Blog Post Handoff — For Opus (QA + Final Edits)

## Background
[1–2 sentences: what the post is, who wrote the draft]

## Why this is escalated to Opus
[What went wrong with Sonnet, if anything. What needs editorial judgment.]

## Task 0: Audit the draft
Read the entire post. Flag any specific claim (number, date, quote, 
sequence of events) that isn't sourced from the handoff or verifiable 
from the attached files. [List known fabrications already caught.]

## Task 1–N: Bounded additions/fixes
[Each task: what to change, where, why, with verified source material]

## Rules
- Do NOT rewrite existing prose. Additions only, at clear insertion points.
- [Exception for specific sections that need replacement, if any]
- If uncertain whether something is verified or fabricated, say so.
- Show the diff before finalizing.
```

### What Opus does well at this stage
- Catching fabricated details by cross-referencing source documents
- Verifying quotes actually exist in cited sources
- Web searching to verify external claims (research citations, dates, statistics)
- Making editorial judgment calls: "This anecdote is strong, this one is filler"
- Writing precise, nuanced additions that require deep understanding
- Saying "I can't verify this" instead of manufacturing confidence

---

## Rules That Apply Throughout

### The merge protection rule

Never tell either model to "merge," "consolidate," or "integrate" new content into existing prose. This triggers rewrites of text that was already good. Instead:

- **Additions only, at named insertion points.** "Add 2 sentences after the paragraph ending with [X]."
- **Replacements only when explicitly marked.** "Replace the paragraph starting with [X] and ending with [Y] with this: [new text]."
- **Treat the draft as read-only** once it's past the structural phase.

### The fabrication check

For every specific claim in the final draft, you should be able to point to either:
- A line in the handoff that contains that fact, or
- A source document that verifies it, or
- Your own direct knowledge (and you've confirmed it's not a false memory planted by a plausible-sounding draft)

If you can't do this for a claim, it's suspect until verified.

### The "show me the diff" rule

Any time you ask either model to edit the draft, require it to show exactly what changed before you accept it. This catches silent rewrites — the model "improving" sentences you didn't ask it to touch.

---

## What This Workflow Costs

For a ~3,000-word blog post:

| Stage | Model | Tokens (approximate) | Time |
|-------|-------|---------------------|------|
| Handoff writing | You | 0 (your time) | 1–2 hours |
| Sonnet draft + 2 revision rounds | Sonnet 4.5 | ~30K in, ~15K out | 30 min |
| Opus audit + final edits | Opus 4.5/4.6 | ~40K in, ~10K out | 45 min |
| Your review and WordPress formatting | You | 0 | 1 hour |

Total model cost on API: roughly $1–3. On Pro/Max subscription: included.
Total your time: 3–4 hours. Most of it is the handoff and final review — the parts only you can do.

---

## Failure Modes to Watch For

1. **Sonnet fabricates examples.** It will. The handoff reduces this but doesn't eliminate it. Every concrete detail in the draft needs verification.

2. **Sonnet defends fabrications when challenged.** If you ask "is this true?" and Sonnet says "yes, it's from the handoff" — verify the handoff yourself. Don't trust the model's report about its own sources.

3. **Opus over-edits.** Opus is thorough but can be heavy-handed. If you say "fix section 2," it may rewrite sections 1 and 3 too. Be specific: "Replace only the paragraph starting with X."

4. **Both models pad transitions.** "This brings us to..." / "It's worth noting that..." / "Perhaps most importantly..." — cut these in your final review.

5. **The draft sounds like Claude, not you.** Both models default to a particular register: warm, slightly formal, structured with clear transitions. If your voice is different, you'll need to do a tone pass yourself. The model can get 80% of the way there; the last 20% is your ear.

---

## The Meta-Lesson

This workflow exists because the blog post it produced is about exactly this problem: LLMs are powerful but unreliable in ways that are silent and cumulative. The drafting model fabricated evidence about fabrication. The QA model caught it — but only because a human knew to escalate.

The user is always the quality control layer. The workflow just makes the QA cheaper and more systematic.

---

_This document is part of the [llm-operational-discipline](https://github.com/mycarta/llm-operational-discipline) repository._
