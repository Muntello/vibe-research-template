# Research Template

This project is a focused investigation of a specific topic. The topic is defined at the start of work.

## Structure

- `sources/` — raw materials: transcripts, articles, links, notes
- `instructions/` — tools and methodology
- `results/` — synthesis and conclusions

## Working with Sources

- Never invent sources. If a source is not in `sources/` and was not shared in the current session — say so.
- When referencing a fact, always cite the source and include a quote:
  > "exact quote from the source" — [title] (file or URL)

### Source intake — two flows

**Flow 1: file dropped into `sources/`**
At session start, scan `sources/` for files not yet listed in `_index.md`. Add each one automatically — no user action needed.

Supported formats:
- `.md` / `.txt` — use as-is
- `.pdf` — extract text and save as `{name}.md` alongside the original
- `.mp3` / `.m4a` / `.wav` / other audio — must be converted to `.md` before processing (see `instructions/tools.md`)

**Flow 2: link or content shared in chat**
When the user shares a URL, text, or file in the conversation:
- If it's a YouTube link → fetch transcript via tool in `instructions/tools.md`, save to `sources/`
- If it's another URL → save as a reference entry in `_index.md` (no local file required)
- If it's pasted text → save to `sources/` as a `.md` file

In all cases: register in `_index.md` and confirm to the user. Never ask the user to update `_index.md` manually.

### Source notes

After analysing a source, save key excerpts and relevance notes to `{source-name}.notes.md` alongside the source file. Structure:
- why this source is relevant to the research topic
- key quotes with timestamps or page references
- ideas to explore further

## Working with Results

- Working synthesis goes in `results/_draft.md`.
- Final documents are separate files in `results/` with descriptive names.
- Clearly distinguish: what the source said vs. what you (Claude) interpret.

## Language

All files (results, indexes, drafts) are saved in **English** by default.
At the start of a research topic, ask the user for their preferred language and record it here.

## Tools

Tool descriptions are in `instructions/tools.md`.
Research methodology template is in `instructions/methodology.md`.

## Maintaining This File

**When to add rules:** if the user corrected the same approach twice — that's a signal to add a rule. A one-off correction does not require an update.

**How to add:** one rule = one line. No explanations, no comments — just the rule.

**Size:** target is under 100 lines. At 120+ lines, propose an optimization session to the user before continuing work.

**How to avoid bloat:** if a rule hasn't been recalled in several sessions — it's a candidate for removal. At 120 lines, discuss with the user what to remove or move to `instructions/`.

## Session Start

At the beginning of each session:
1. Read `sources/_index.md` — what has already been processed
2. Read `results/_draft.md` — where we left off
