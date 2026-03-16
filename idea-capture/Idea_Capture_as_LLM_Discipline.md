# Idea Capture as LLM Operational Discipline

## The Problem

During sustained human-LLM collaboration, tangential ideas surface constantly. The model detects them — it says "interesting, let's file that for later" or "that's a thread for another post" — and steers the conversation back to the task at hand.

This tangent detection is reliable. The model provides consistent 20/20 vision on when a conversation has drifted from its stated goal.

But "let's file that for later" is a speech act, not an action. The idea lives only in the conversation transcript. The transcript may never be reread. Context compaction may discard it. The next session may not recover it. The model's tangent detection produces polite redirection and nothing else.

The result: good ideas that surfaced during focused work are lost — not because nobody noticed them, but because noticing had no mechanism behind it.

## The Principle

**When an LLM flags a tangent during focused work, the redirection must have a destination. An idea noted as "for later" with no capture mechanism is an idea lost to transcript decay.**

The model already provides the detection. The infrastructure to act on it is the missing piece.

## Why This Is LLM Discipline, Not Human Discipline

The recognition of tangents during collaborative work is not the bottleneck. The human opens the door; the model flags it and steers back. That steering behaviour is natural to the LLM — it's optimising for task completion.

The discipline failure is operational: there is nowhere to steer the idea *to*. Without a capture system, the human faces a bad choice every time something interesting surfaces:

- **Follow the tangent** → lose focus, blow the session budget, derail the primary task
- **Ignore it** → lose the idea to transcript entropy

A capture system introduces just enough friction to **note without pursuing**. It turns the model's natural steering behaviour into a concrete action rather than a conversational gesture.

## What a Capture System Needs

The requirements are minimal:

1. **Cross-session persistence.** The captured idea must survive beyond the conversation that produced it. Handoff documents, memory notes, and in-chat acknowledgments do not meet this bar — they are subject to the same transcript decay they're meant to prevent.

2. **Cross-project reach.** Ideas surface in whichever conversation happens to be active. The capture mechanism must work regardless of which Claude project, chat, or tool the human is using at the moment.

3. **Low friction.** If capturing an idea takes more than ~15 seconds, the human will choose "ignore it" every time. The system must be faster than the tangent it's capturing.

4. **Model-generatable.** The model should be able to produce the capture artifact (a structured note, a card, a formatted entry) on request, so the human's only job is to trigger the capture and confirm it landed.

## One Concrete Implementation

**Stack:** A single HTML file on your desktop + Trello REST API. No middleware, no webhooks, no dependencies.

**Flow:**
1. During conversation, a tangent surfaces
2. Human says: "Give me a capture card for this idea"
3. Model produces a structured JSON snippet (title, one-line description, source project, status tag)
4. Human pastes the JSON into a lightweight HTML form saved on their desktop (or fills in the form fields directly)
5. Form POSTs directly to Trello's API → card is created in the target list
6. Idea has an address. Conversation continues on task.

**JSON schema (example):**
```json
{
  "title": "Short title of the idea",
  "description": "One or two sentences — what the thread is and why it matters.",
  "status": "Seed",
  "url": "https://claude.ai/project/[project-id]"
}
```

The HTML tool, setup instructions, and a template with placeholder credentials are included in this directory. See `README.md`.

This is one implementation. Others are equally valid: a shared note in Apple Notes or Obsidian, a GitHub issue, a voice memo, a dedicated Slack channel. The mechanism matters less than the existence of a destination.

## What This Does Not Solve

The capture system addresses operational discipline — giving the model's tangent detection somewhere to land. It does not address the deeper question of **human input discipline (L0)**: whether the human examined the idea before it became a tangent, whether the tangent reflects an unchecked bias or an unexamined assumption, or whether the accumulation of captured ideas becomes its own form of productive procrastination.

A full inbox of seed cards is not the same as a writing plan. Capture without triage is hoarding.

The capture system is infrastructure for a habit. Whether the habit serves the work depends on what happens after the card lands.

## Origin

This principle was identified during a sustained multi-project Claude collaboration (January–March 2026). The pattern — model flags tangent, says "for later," idea is lost — was observed repeatedly across sessions before being named. The Trello/Make implementation was designed in a separate project (March 8, 2026) and documented in a handoff report. The principle was extracted for this document on March 15, 2026.

The irony that this system for capturing escaped ideas was itself an escaped idea that needed to be captured is noted in the original handoff.
