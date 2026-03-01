# Architecture Mapping: Current System → Claude Code

**Date:** 2026-03-01
**Context:** Shane Butler's "Will Claude Code + Opus 4.6 Replace Your Data Team?" (Data Neighbor, 2026-02-16) describes an architecture for analytical workflows in Claude Code that independently converges on the same structural pattern used in this project's research methodology system.

---

## The Parallel

Butler's system uses four components:

- **CLAUDE.md** — project-level persona, scope, and rules. Read automatically when Claude Code starts.
- **Skills** — always-on standards that apply whenever a trigger condition matches (visualization patterns, data quality checks). You don't invoke them; they fire automatically.
- **Agents** — multi-step workflows invoked on demand with defined inputs, steps, and outputs.
- **Rules** — non-negotiable guardrails: "never present unvalidated findings as conclusions."

This maps directly to the current research methodology system:

| Butler's Claude Code | This project (Claude Projects) | Function |
|---|---|---|
| CLAUDE.md | Research_Project_System_Prompt_v3.md | Persona, scope, workflow, standing rules |
| Skills | faithful_narration_rules.md | Always-on standards that apply to all output |
| Agents | The 5-step workflow (Scope → Gather → Report → Review → Summary) | Multi-step process with defined gates |
| Rules | Standing Rules + Prohibited Phrases | Non-negotiable guardrails |
| Correction → config update | Failure → new rule (e.g., Rule 20 from connotation-level fabrication) | Compounding memory |
| Validation agent | Decision Checkpoints + Self-Check pass | Human-in-the-loop verification |

The compounding corrections loop is the deepest parallel. Butler describes feeding back presentation errors (chart title duplication, narrative voice, annotation collisions) into the agent and skill files so they never recur. This is the same pattern that produced the faithful narration rules: each rule traces to a specific, documented failure mode encountered during real work. Every failure becomes a permanent guardrail.

---

## Where It Diverges: The Error Surface

Butler's system optimizes for throughput and presentation quality. This project optimizes for epistemic reliability. The difference matters because the two domains have fundamentally different error surfaces.

**Butler's errors are mostly syntactic** — wrong numbers, bad formatting, duplicated titles, chart annotation collisions. These are detectable by inspection or by automated checks. His validation agent re-derives key numbers independently and cross-checks arithmetic. The Simpson's Paradox check catches hidden segments automatically.

**This project's errors are mostly semantic** — contextual literature cited at direct-evidence weight, source access misrepresented as full-text review, connotation-level fabrication through word choice, secondary sources cited without checking primaries. These require domain expertise to detect. No automated validation step catches "this paper is real and related but doesn't demonstrate what it's being used to claim."

Butler's pipeline would catch "the number is wrong." It's not clear it would catch "the number is right but the source doesn't demonstrate what it's being used to claim."

The corrections also compound differently. Butler's corrections are about presentation — visible on inspection. This project's corrections are about epistemology — requiring domain expertise to catch in the first place. The compounding only works if someone identifies the failure, and semantic failures are harder to identify than syntactic ones.

---

## Implications for Migration to Claude Code

If this research methodology were moved into Claude Code, the mapping would be:

**CLAUDE.md** ← Research_Project_System_Prompt_v3.md (persona, scope, workflow gates, standing rules, prohibited phrases)

**Skills directory:**
- `faithful_narration.md` ← faithful_narration_rules.md (trigger: any content drafted in the user's voice)
- `source_classification.md` ← Source Classification section from system prompt (trigger: any citation)
- `evidence_grading.md` ← Direct / Analogical / Contextual definitions (trigger: any source introduced as evidence)

**Agents directory:**
- `scope_question.md` ← Step 0 (invoke at project start)
- `gather_sources.md` ← Step 1 including source inventory table (invoke after scope agreed)
- `prepare_report.md` ← Step 2 with citation rules and self-check (invoke after sources in hand)
- `user_review.md` ← Step 3 (invoke after report delivered)
- `executive_summary.md` ← Step 4 (invoke after user returns finalized report)

**Key advantage of modularization:** Currently the system prompt is monolithic — all rules, all workflow steps, all classification systems in one document. Butler's architecture separates what's always on (skills) from what's invoked on demand (agents). This means the faithful narration rules would load into context whenever Claude drafts content in the user's voice, but the source gathering workflow wouldn't consume context tokens during the writing phase. The source classification system would activate when citations appear, not when drafting prose.

**Key risk of modularization:** The monolithic prompt ensures all rules are visible simultaneously. Splitting into separate files means Claude Code loads them selectively based on trigger conditions. If a trigger doesn't fire, the rule doesn't apply. For research methodology where the failure modes are subtle and interconnected (a source classification error causes a faithful narration violation downstream), missed triggers could reintroduce failure modes the rules were built to prevent.

---

## Confirmation from the Anthropic Team's Own Workflow

A LinkedIn post summarizing the Claude Code founder's team workflow (10-step setup) provides additional data points. Most of it is developer infrastructure (parallel worktrees, terminal setup, CI debugging) that doesn't transfer to research methodology. Three points do.

**"Treat CLAUDE.md like a memory system."** The team's practice: after every correction, tell Claude to update CLAUDE.md so it doesn't repeat the mistake. They report that mistake rates drop over time with ruthless editing. This is exactly the pattern that produced the faithful narration rules — each rule traces to a caught failure. Independent confirmation from the model's own development team that the compounding corrections loop works.

**"Start complex tasks in plan mode."** The team uses one Claude session to write the plan and a second to review it as a staff engineer. For research methodology, the equivalent: one session builds the source inventory and argument structure, a second stress-tests evidence grading and source classifications before drafting. This extends the existing Sonnet-drafts-Opus-QA blog pipeline to the research analysis phase itself — not just the writing phase.

**"Use Claude as a harsh reviewer."** Team prompts include "grill me on these changes" and "prove this works." This maps to Standing Rule 7 ("when challenged, re-examine rather than defend") and to the VolZug re-grading session that triggered the entire methodology. Notable that the team that built the model still has to explicitly prompt for adversarial review — the sycophancy default doesn't go away.

---

## The Open Question

Butler's post doesn't describe what happens when the pipeline produces confident output that's substantively wrong — not a chart error or formatting issue, but an analytical conclusion that doesn't hold up. Either it hasn't happened yet, or it happened and wasn't caught, or it was caught and he didn't write about it.

The first would be surprising. The second is the risk this project's methodology is designed to address. The third would be the most interesting thing he could write about.

The fundamental difference: a wrong chart title is embarrassing. A wrong evidential weight assessment can mislead policy.

---

## References

- Butler, S. (2026). "Will Claude Code + Opus 4.6 Replace Your Data Team?" *Data Neighbor Newsletter*, February 16, 2026. https://dataneighbor.substack.com/p/will-claude-code-opus-46-replace
- Butler, S., Madipalli, S., & Guan, H. (2026). "Build AI Analysts in Claude Code" bootcamp. Maven. https://maven.com/dataneighbor/build-ai-analysts-in-claude-code
