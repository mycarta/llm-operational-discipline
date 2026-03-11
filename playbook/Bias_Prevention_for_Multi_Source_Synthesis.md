# Bias Prevention for Multi-Source Synthesis

**Prompt-level guardrails for LLM-assisted synthesis across unequal sources**

---

## The Problem

When an LLM synthesizes content from multiple sources that differ in length, sophistication, or format, the default output systematically over-represents verbose, technically polished submissions and under-represents shorter or simpler ones. This isn't a hallucination problem — the model faithfully reflects what it received. The bias is structural: more tokens in, more influence out.

This matters in any workflow where multiple voices need fair representation: stakeholder consultations, literature reviews, survey synthesis, multi-team retrospectives, competitive analysis, policy comment summaries, peer review aggregation.

Five rules, embedded directly in synthesis prompts, counteract the most common distortions.

---

## The Five Rules

### 1. Verbosity Normalization

**Failure mode:** A 30-page source with 12 points on a topic gets 6× the coverage of a 3-page source with 2 points on the same topic, even when both positions carry equal weight.

**The rule:** Each source's position is represented by its substance, not its word count. Theme headings and section length are driven by how many sources raised an issue, not how much any one source wrote about it.

**Prompt language:**
> Represent each respondent's position by substance, not volume. A lengthy submission does not receive proportionally more synthesis coverage than a concise one making the same point.

### 2. Category-Stratified Counts

**Failure mode:** Aggregated respondent counts obscure the composition of support. "Six respondents raised this concern" could mean six from one stakeholder category, or three categories each contributing two — these have very different implications for how broadly held a position actually is.

**The rule:** Report respondent counts by source category. Define categories appropriate to your domain — organizational type, expertise level, stakeholder role, sector, geography — and always disaggregate.

**Prompt language:**
> When reporting how many respondents raised a theme, always disaggregate by source category (e.g., "three of five industry respondents and two government respondents" rather than "five respondents"). Define source categories in the input data.

### 3. Absence Tracking

**Failure mode:** When a source doesn't address a topic, the synthesis simply omits that source from the discussion. Readers then have no way to distinguish between "didn't comment" and "implicitly agreed." Silence becomes invisible, which biases interpretation toward apparent consensus.

**The rule:** Every theme section explicitly notes which sources (or source categories) did not address that topic. Silence is data. Treating it as such prevents readers from inferring support or opposition where none was expressed.

**Prompt language:**
> For each theme, note which source categories did not address the topic. Do not allow silence to be read as implicit agreement or disagreement.

### 4. Intensity Preservation

**Failure mode:** LLMs flatten source language toward a neutral register. Strong positions ("unacceptable," "critical flaw," "non-negotiable requirement") get softened to "concerns were raised." Hedged positions ("may warrant consideration," "could be explored") get inflated to definitive recommendations. Both distortions misrepresent the source.

**The rule:** The synthesis preserves the intensity of source language. Strong language stays strong. Hedged language stays hedged. The QA pass should specifically check for intensity drift in both directions — inflation of hedges is at least as common as softening of strong positions.

**Prompt language:**
> Preserve the intensity of source language. If a source uses strong language (e.g., "unacceptable," "essential"), reflect that strength. If a source hedges (e.g., "could be considered," "may warrant"), preserve the hedge. Do not flatten strong positions to neutral summary or inflate tentative positions to definitive ones.

### 5. Confidentiality Discipline

**Failure mode:** Sources with restricted distribution (confidential submissions, embargoed data, internal-only documents) leak into public-facing synthesis through paraphrase or thematic influence. Alternatively, public outputs are written in a way that only makes sense if the reader has also seen the restricted material.

**The rule:** Restricted sources may inform internal theme identification but cannot appear in public outputs. The public synthesis must stand on its own using only publishable content. Where restricted material influenced theme selection, an internal-only note flags this dependency.

**Prompt language:**
> [List restricted sources by name.] These sources may inform your understanding of themes but must not appear in the synthesis output — no paraphrases, no attributions, no positions that are only intelligible with knowledge of restricted content. The output must be self-contained using non-restricted sources alone.

**Note:** This rule may not apply to every synthesis project. Include it when your source set contains any access-restricted material — embargoed data, confidential submissions, privileged communications, pre-decisional drafts.

---

## Implementation

### Embed in the prompt, not the review

These rules belong in the synthesis prompt itself, not as a post-hoc checklist. LLM outputs are far easier to shape at generation time than to fix after the fact. Treating bias prevention as a review-stage activity means you're editing a biased draft rather than preventing the bias from forming.

### Pair with a QA audit

Even with prompt-level guardrails, the synthesis will occasionally drift. A QA pass should specifically check:

- **Verbosity normalization:** Is any single source dominating a theme section? Does the length of coverage correlate with source length rather than number of sources?
- **Count disaggregation:** Are respondent counts reported by category, or are they collapsed into undifferentiated totals?
- **Absence reporting:** Does every theme section identify who didn't comment? Are there sections where silence is invisible?
- **Intensity fidelity:** Compare synthesis characterizations against source language. Flag any instance where strong language was softened or hedged language was inflated. (LLMs tend to inflate hedges more than they soften strong positions — look for this asymmetry.)
- **Confidentiality leakage:** Can the public output stand alone without knowledge of restricted sources?

### Adapt categories to your domain

The five rules are domain-agnostic, but the source categories for Rule 2 are not. Define them based on what matters for your synthesis:

| Domain | Example categories |
|---|---|
| Policy consultation | Industry, government, Indigenous, NGO, public |
| Literature review | Empirical study, review article, grey literature, preprint |
| User research | Power user, occasional user, churned user, prospect |
| Multi-team retro | Engineering, product, design, QA, leadership |
| Competitive analysis | Direct competitor, adjacent market, emerging entrant |

### Scale to source count

For small source sets (< 10), absence tracking and category-stratified counts are straightforward. For large sets (50+), you may need to batch by category first, synthesize within categories, then synthesize across categories — otherwise the LLM's context window becomes the binding constraint and verbosity bias reasserts itself through token-budget competition.

---

## Origin

These rules were developed during an LLM-assisted synthesis of stakeholder submissions for a government regulatory consultation. The source set included submissions ranging from short form responses to lengthy technical briefs, spanning multiple stakeholder categories with very different levels of resources and sophistication. Without these guardrails, pilot synthesis runs systematically over-represented the longest, most technically polished submissions. With them, plus a QA audit trail, several instances were identified where the synthesizer's characterization language overstated source positions despite the prompt-level rules.

The rules are prompt-level instructions, not post-hoc editorial judgment. They were designed to be embedded in any synthesis prompt and to work with any capable LLM.

---

## License

MIT — see [LICENSE](../LICENSE)

## Author

**Matteo Niccoli**
- Blog: [mycartablog.com](https://mycartablog.com)
- LinkedIn: [Matteo Niccoli](https://www.linkedin.com/in/mniccoli/)
