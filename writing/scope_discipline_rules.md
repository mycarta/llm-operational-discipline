# Scope discipline

A companion to the faithful narration rules. Those govern what a claim
may say. This governs how wide a claim may reach.

## The failure

A check is run at one scope. A claim is made at a wider one. The check
is real, the claim is not, and the gap between them is invisible in the
output because both are stated in the same confident register.

Four instances from a single working day, all by the same agent, all
caught by the human rather than by the agent:

1. A semantic search for a file returned nothing. Claim: the file is
   absent from project knowledge. A search failing to return something
   is not evidence that it does not exist.
2. A number appeared twice with different values in the current
   documents. Claim: this may be new drift. Novelty is a fact about
   history. It cannot be established from the current documents.
3. Several rows of a status table were re-checked against primary
   artifacts. Claim, in the document header: every row was re-verified.
   The rows not checked were carried forward silently and one of them
   was then escalated with urgency language.
4. A dispatch dated eleven days earlier described a contingency plan.
   Claim: this is the current plan. A dated document is evidence about
   its date.

A fifth, later the same day, after the rule below had already been
articulated: a filesystem listing of a mounted directory showed two
files absent. Claim: they are absent from project knowledge. The mount
and the system are not the same thing. The human re-uploaded both files
on the strength of the claim, creating duplicates.

## The rule

**A claim may not be wider than the check that supports it.**

In practice this decomposes into two obligations.

**1. No global verification claims.** Never write that a document has
been fully verified, fully re-checked, or fully reconciled. Verification
attaches to items, not to documents.

Every status cell, row or asserted fact carries its own provenance
marker:

- `[V 2026-07-27: <artifact>]` verified today against a named artifact
- `[C <source document>]` carried forward from a named prior document,
  not re-checked
- `[U]` unverified, asserted without a source

Then the claim is per item and cannot overreach. The reader scans for
`[C]` and `[U]` and spot-checks those. Nobody has to hold the whole
document in their head.

Applied retroactively to instance 3 above, the row would have read
`[C dimension_gap_list_20260721b.md]`, and neither party would have
believed it was checked.

**2. State the scope of a negative.** Negative claims are where the gap
opens widest, because absence of evidence reads as evidence of absence.
Say what was checked, not what is true.

- Not "the file is absent." Say "not returned by search of X" or "not
  present in the mounted directory at 17:22."
- Not "this is a new discrepancy." Say "not found in the documents I
  read; prior sessions not searched."
- Not "there is no record of it." Say what was searched.

If the wider claim actually matters, run the wider check first. A file
listing settles existence in the mount. Only the human or the system of
record settles existence in the system.

## Why this rather than more care

The five instances above are not five different lapses of attention.
They are one mechanism in five costumes, which is why the pattern
survives resolutions to be more careful. Care does not fix it because
the agent is not being careless: each check was genuinely run. What
fails is the silent widening between the check and the sentence.

A rule that depends on vigilance transfers the cost to the human
auditor, who is the scarcest resource in the loop and the one already
doing the catching. This rule instead removes the sentence form that
makes the error expressible.

## Test

Greppable. Search a produced document for `every`, `all rows`, `fully`,
`no record`, `does not exist`, `is absent`. Each hit is either a claim
whose check covered that scope, or a defect.

Provenance markers are greppable by construction: `[C ` and `[U]`
enumerate everything in the document that is not verified.
