# Multi-Agent LLM Document Review: Practitioner Lessons

Patterns and failure modes observed across a multi-month, multi-agent LLM-assisted review of ~25 regulatory submissions. One LLM handled extraction and summarization; a second handled prompt design, validation, and synthesis. Submissions ranged from 3-page letters to 38-page technical narratives in formats including web forms, narrative PDFs, email attachments, and slide decks.

These lessons are domain-transferable. The project happened to be regulatory, but the patterns apply to any structured extraction from a heterogeneous document corpus.

## Architecture: Why Two Agents

The workflow uses two LLMs in distinct roles: one extracts, the other validates and synthesizes. This isn't an accident or a resource constraint — it's a deliberate design choice rooted in how LLMs fail.

A single agent asked to extract and then validate its own output will confirm its own work. It has no independent basis for noticing omissions because the same context window, attention patterns, and sense of "completeness" that produced the original extraction also govern the review. Self-validation converges on consistency, not correctness.

Splitting the roles introduces genuine independence. The validator works from the source document and the extraction output as separate inputs, with a different model, different context, and a different prompt framing (adversarial rather than generative). It can ask: "Is this everything?" without being anchored to the first agent's sense of "enough."

The division also enables role-appropriate prompt design. The extraction prompt optimizes for recall: find everything, quote verbatim, preserve structure. The validation prompt optimizes for precision and completeness: verify quotes against source, check for gaps, enforce schema constraints. Trying to do both in a single prompt creates competing objectives.

This mirrors a pattern that works in human workflows too — the person who drafts a regulatory filing is not the best person to QA it. Fresh eyes catch different things. The same principle applies to LLMs, possibly more so, because their failure modes are more systematic.

---

## Extraction

**LLMs are better at finding what's there than noticing what's missing.**  
The dominant failure mode across every document was omission, not error. Verbatim quotes were nearly always accurate. But the model would extract a coherent subset of content and stop — even when the source contained significantly more material. Dense documents (20+ pages) consistently lost 15–60% of extractable items on first pass. The model's sense of "done" is calibrated for conversational completeness, not domain completeness. Explicit instructions like "multiple content items expected per section" help but don't fully solve it.

**Self-contained prompts beat chained prompts for accuracy-critical work.**  
Multi-step prompt chains and notebook-style context persistence introduced subtle corruption — JSON malformation, category drift, hallucinated field names. Switching to a single self-contained prompt (every instruction the model needs in one message) eliminated these failure modes. The prompt is longer but the output is reliable. For work where output integrity is non-negotiable, resist the temptation to build elegant pipelines.

**One document, one fresh session.**  
Processing multiple documents in a single LLM session introduced context drift — the model applied framing, terminology, or categorization decisions from Document A when processing Document B. Each document must be evaluated on its own terms. The time cost of re-pasting the prompt is trivial compared to the QA cost of untangling cross-contaminated extractions.

**Match the tool to the document.**  
Dense 30+ page narratives benefit from a full extraction pipeline. Short documents (under ~8 pages) and visual/slide-based submissions don't — direct extraction by the validator in a single pass is faster and more complete. Running a sparse slide deck through a prompt designed for continuous prose wastes tokens and introduces forced-fit artifacts.

---

## Schema Design

**Constrain taxonomies to a closed set and enforce it explicitly.**  
If your schema defines three content types (e.g., fact/position/concern), the LLM will invent a plausible fourth (e.g., "recommendation") unless explicitly told not to. Add a hard instruction: "Content type values must be one of: [list]. Do not introduce other types." Catching taxonomy drift early prevents downstream categorization chaos in synthesis.

**Distinguish the respondent's voice from quoted authority.**  
In any document that responds to prompts or questions from an authority (a regulator, a funder, an RFP), the model's default is to extract the most authoritative-sounding text as the "key quote" — which is often the authority's own language, not the respondent's answer. Add an explicit instruction to extract the respondent's position, not parroted source material.

**Design for content that crosses boundaries.**  
If your extraction framework has defined categories (topics, questions, themes), some content will genuinely span multiple categories. A dual-layer structure works well: (1) bucketed content for material clearly addressing a specific category, and (2) cross-cutting arrays for background, general positions, and concerns that span categories. The cost of a parallel array is low; the cost of forced single-bucket assignment is false precision.

---

## QA

**Two verification dimensions catch different failure modes.**  
Vertical: traverse all categories for one document (catches omissions within a document). Horizontal: traverse one category across all documents (catches inconsistent bucketing, uneven extraction depth, and missed convergence). These are not redundant — they find different problems. Plan for both.

**Visual verification is non-negotiable for image-based PDFs.**  
Image-based PDFs (scanned, print-to-PDF, embedded-image layouts) defeat text extraction silently. You think you have the content; you don't. Render pages as images at sufficient resolution and compare quotes visually. In Python, `page.get_pixmap(matrix=fitz.Matrix(2.0, 2.0))` via PyMuPDF is reliable.

**Tag every patched item with provenance.**  
When the validator adds or corrects items, tag them with a `post_qa_note` field (what, why, who). Add an `extraction_metadata` block to each output file (base tool, prompt version, scope, deviations). The marginal cost is seconds per item. The value when someone asks "where did this come from?" months later is enormous.

---

## Workflow

**Session handoff files are the single most important practice.**  
LLM context windows compact or reset. Conversations end. The numbered session handoff file (Session_Handoff_N.md) records what was completed, what was decided, what's next, and what files were produced. Write one at every natural breakpoint. It takes 5 minutes and saves hours of reconstruction. Without it, every compaction or new session is a cold start.

**One living tracker file is the source of truth.**  
A single markdown file tracking every document's status: extracted or not, which tool, QA status, output file names, consent/confidentiality, flags, deviations. Updated immediately after each extraction. Prevents duplicate work, catches gaps, enables status reporting. Don't rely on memory, session history, or file listing.

**Check access restrictions at extraction time, not output time.**  
If your corpus includes documents with different access levels (public, confidential, restricted), track this in the extraction output itself — not in a separate spreadsheet you'll forget to check. Bake it into the schema as a field. If you wait until the synthesis or publication stage, you risk contaminating public outputs with restricted content.

---

## LLM Behavior

**The undercapture pattern is structural, not random.**  
LLMs generate until they reach a sense of conversational completeness. For a 30-page document with 40 extractable items, a well-structured 15-item extraction *feels* complete — it has coverage across categories, representative quotes, and a coherent narrative. The remaining 25 items aren't harder to extract; the model just stopped looking. This is the single most important pattern to internalize: first-pass extraction from dense documents is a floor, not a ceiling.

**Anthropomorphization creates false expectations.**  
Phrases like "as we discussed" or "send me the file" across sessions imply persistent memory and relationship that don't exist. Each session (and sometimes each message after context compaction) is a fresh start. Workflow documentation should describe LLM interactions as they actually work: session-scoped, context-window-limited, dependent on explicit state files. This isn't pedantry — it prevents real mistakes when people assume the model "remembers."

---

## Synthesis

**Convergence across independent sources is the highest-value signal.**  
The most actionable findings in a multi-document review aren't in any single document — they're in the convergence patterns across documents that didn't coordinate with each other. When five independent actors identify the same constraint from completely different disciplinary perspectives, that's a signal no individual reviewer catches by reading documents sequentially. Extraction is necessary but not sufficient; cross-document synthesis is where the value lives.

**Second-order effects live in the gaps between documents.**  
One submission flagged that if development displaces one ocean user from a specific area, that user might relocate into another user's established route — creating a new conflict neither party caused. This kind of displacement interaction only becomes visible when you have the full corpus and can reason across stakeholder categories. Design synthesis prompts to look for interaction effects between documents, not just within them.

---

*Distilled from a 31-session multi-agent LLM workflow (2025–2026).*
