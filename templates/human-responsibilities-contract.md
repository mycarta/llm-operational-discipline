# Human Responsibilities Contract — [YOUR_NAME]

**Companion to:** `[PROTOCOL_FILE]` (v[VERSION])
**Drafted:** [DATE]
**Scope:** Your bilateral commitments under the protocol. The LLM's hard
commitments are in the protocol; this document is what you commit to in
exchange.

---

## Why this exists

The protocol works only if both sides hold their end. The LLM's failures become
visible in file artifacts only if you audit the artifacts. The protocol's
core constraint — "the LLM cannot enforce its own behavior" — has its mirror:
**audit mechanisms produce nothing useful unless audited.**

This document is your duty to the team. The LLM requires every item below.
None are optional.

---

## Cognitive presence — non-negotiable

**Do not work [PROJECT_NAME] when cognitively impaired.**

Specifically: do not open a session when —

- Tired (post-midnight without sleep, after a long workday, end-of-day fatigue)
- Emotionally compromised (heated, anxious, distracted by other crises)
- Time-pressured to a degree that rushes audits (<30 min available, deadline-eve panic)
- Deep in another project's cognitive context ([OTHER_PROJECT_1], [OTHER_PROJECT_2]) without a clear reset
- After alcohol or anything else that degrades judgment

**Why this is a hard non-negotiable:** the historical narrative-drift incidents
([DOCUMENTED_FAILURE_1], [DOCUMENTED_FAILURE_2], [DOCUMENTED_FAILURE_3]) were
all caught by you, not by the LLM. If your audit capacity drops, the system
has no fallback. LLM drift propagates uncaught.

If a session opens and any of the above is true: write a one-line note to
`[STATE_FILE]` ("session aborted [DATE]: cognitive state insufficient,
deferred to <when>") and close the chat. Resume when present.

---

## Session-opening duties

When the LLM outputs `[STATE_FILE]` contents and the confirmation message:

1. **Read the contents.** Actually read. Not skim.
2. **Spot-check at least one citation.** Pick a value, ask the LLM to point to
   the source file, open the file, verify.
3. **Confirm or push back.** "Activated, proceed" or specific objection.

If unwilling to do step 2, the protocol is dead-letter. Don't open the session.

---

## In-session duties

**Audit citations actively.** Per protocol §[CITATION_RULE]: every numeric
value gets a citation. If the LLM states a value without citing, point it out.
Don't let it slide. Each unflagged uncited value erodes the system.

**Demand quotes for authority claims.** Per protocol §[AUTHORITY_RULE]: when
the LLM cites a source, ask for the verbatim quote. If it can't supply one,
the citation is bare authority-borrowing — flag it.

**Push back on stage skipping.** If the LLM is in Stage 1 and starts answering
Stage 2 questions, redirect. Stage discipline only works if both sides respect it.

**Don't say "we discussed earlier."** Reference files, not memory. If it's not
in a file, it didn't happen.

**Call context degradation.** If the LLM is hallucinating, drifting, or losing
thread mid-session: say so. Trigger the compaction pre-flight. Don't wait for
the LLM to notice.

---

## Stage-gate duties

When the LLM reports a stage complete:

1. Open `[STAGE_HANDOFF_FILE]`.
2. Verify every listed gate has an observed value and pass/fail.
3. Verify the outputs section lists real files (open at least one).
4. Approve explicitly: **"Stage [N] approved, handoff immutable"** — or reject
   with specific objection.

"OK" is not approval. Approval is the explicit phrase.

---

## Session-closing duties

Before closing the chat:

1. Confirm the LLM updated `[STATE_FILE]`.
2. If a stage completed: confirm the immutable handoff was written.
3. Confirm any numeric value mentioned in chat appears in `[STATE_FILE]` or
   the handoff, with citation.

Closing without these = silent state degradation between sessions.

---

## Hard commitments — what you will NEVER do

Mirror to the LLM's hard commitments. Each is a specific, auditable failure on your part.

✗ Open a session when cognitively impaired (see above).
✗ Skip the session-opening audit ("just continue from last time").
✗ Accept an uncited numeric value from the LLM without flagging.
✗ Override a [PROTOCOL_STOP_FLAG] with "it's fine, just give me the answer."
✗ Pressure the LLM to skip ahead past a stage gate.
✗ Edit `[STATE_FILE]` or any stage handoff outside an active session.
✗ Reference past discussion without pointing to its file location.
✗ Approve a stage handoff without opening the file.
✗ Close a session without confirming `[STATE_FILE]` was updated.
✗ Bypass the protocol with "informally just this once."

---

## Failure-mode self-checks (signs to abort a session)

If any of the following happen during a session, abort cleanly: update
`[STATE_FILE]` (or write a compaction snapshot), close the chat, resume later.

- Reading the LLM's output but not actually parsing it.
- Saying "ok" to things not verified.
- Asking the LLM to "just continue" past flags.
- Autopilot: pattern-matching responses instead of thinking.
- Deadline pressure leading to skipped audits.
- Catching yourself rationalizing why a shortcut is fine.

These are not character flaws. They are signs the brain is no longer capable of
running the audit loop. Abort, defer, resume present.

---

## Bandwidth honesty

If a session opens and:
- <30 minutes available, AND
- the work needs >30 minutes,

then defer. A 30-minute session producing an unverified handoff is worse than
no session.

Better: 15-minute session that updates `[STATE_FILE]` with current state
and explicitly defers the analytical work.

---

## What you get in exchange

The protocol gives you:
- A single source of truth you can trust
- Visible drift instead of invisible drift
- Stage anchors that survive compaction
- A self-audit loop that scales beyond your memory
- A defensible methodology trail for the project itself

The cost is the duties above. The exchange is asymmetric in the LLM's favor
unless you hold your end.

---

## Activation

This document takes effect alongside `[PROTOCOL_FILE]` v[VERSION].

Activation requires your explicit acknowledgment. Format:

> [YOUR_NAME]'s responsibilities document v[CONTRACT_VERSION] acknowledged. Duties accepted as listed.

Until that acknowledgment: working assumption is "duties not yet accepted, normal collaboration applies."

---

## Placeholder reference

| Placeholder | Replace with |
|---|---|
| `[YOUR_NAME]` | Your name |
| `[PROJECT_NAME]` | The name of the project (e.g. "the Monarch Report") |
| `[PROTOCOL_FILE]` | Filename of the companion protocol document |
| `[VERSION]` | Protocol version number |
| `[DATE]` | Date this contract was drafted |
| `[STATE_FILE]` | The persistent project state file the LLM maintains |
| `[STAGE_HANDOFF_FILE]` | Naming convention for stage handoff files |
| `[CITATION_RULE]` | Protocol section that governs citation requirements |
| `[AUTHORITY_RULE]` | Protocol section that governs authority-claim quoting |
| `[PROTOCOL_STOP_FLAG]` | The hard-stop flag name used in your protocol (e.g. "LAW ZERO") |
| `[OTHER_PROJECT_1]` | Names of other projects that create cognitive context-switching risk |
| `[DOCUMENTED_FAILURE_1]` | Short labels for documented drift/failure incidents in your project history |
| `[CONTRACT_VERSION]` | Version number of this responsibilities document |

---

## Revision log

- **[DATE] v1.0** — Initial draft. Companion to `[PROTOCOL_FILE]` v[VERSION].
  Activation pending explicit acknowledgment.
