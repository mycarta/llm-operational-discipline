Document Recovery & Project Audit Prompts

1. Two-Step Merge Recovery (when Claude damaged existing prose during a merge)
Step 1: Diff only
I have two versions of a document. old_version is the good base. merged_version was created by merging new content into it, but the merge damaged existing prose.

Do this in order:

Diff only. List every block in merged_version that does NOT appear in old_version — new sections, new figures/captions, new paragraphs. Quote the first and last line of each block.
Flag rewrites. List every section where old_version text was rephrased, restructured, or shortened. Show old vs. new side by side.
Stop. Do not produce a merged document yet. Wait for my review.
Step 2: Surgical insert (after reviewing the diff)
Take old_version as the base. Insert these blocks: [list]. At these locations: [list]. Do not modify any existing text.

Critical: never let Claude do the diff and the merge in one shot.


2. Project Audit Prompt (for any project with potential degradation)
I want to audit this project for context health. Please:

List every file in /mnt/project/ with approximate size. Flag any superseded, duplicated, or not needed in every chat.
Check /mnt/transcripts/ — how many exist? (More transcripts = more compactions = more potential corruption.)
Read main state/handoff file and flag anything vague, contradictory, or suspiciously round-numbered — compaction artifacts.
Recommend which files to retire and what lean project file set would look like.


3. Prevention Rule (add to Settings → Profile)
Never rewrite existing prose during a merge or consolidation. Additions only, inserted at named anchors. Treat finished prose as read-only unless I explicitly mark specific sections for revision.


4. Fabrication Verification (when Claude defends a challenged claim)

When you challenge a specific detail and Claude responds with quotes or citations from source documents to defend it:

> I'm verifying your last response. Show me the exact location in [source file/document] where [the claimed quote or fact] appears. Use the view tool to read the file and quote the relevant lines with line numbers.

If Claude can't produce the exact lines, the citation was fabricated. Remove the claim or verify it yourself against the primary source.

**The pattern:** Layer 1 is confabulation (inventing details) — expected, buildable QA around. Layer 2 is fabricating provenance when challenged (inventing quotes from source documents to defend the confabulation). Layer 2 is caught only by requiring the model to show its work against the actual file, not by asking "is this true?"


