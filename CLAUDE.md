# Research Template

This project is a focused investigation of a specific topic. The topic is defined at the start of work.

## Structure

- `sources/` — raw materials: transcripts, articles, links, notes
- `instructions/` — tools and methodology
- `results/` — synthesis and conclusions

## Working with Sources

- Never invent sources. If a source is not in `sources/` — say so.
- When referencing a fact, always cite the source and include a quote:
  > "exact quote from the source" — [title] (file or URL)
- New source → add to `sources/_index.md` first, then use it.

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
