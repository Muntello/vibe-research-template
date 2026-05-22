# Research Template

> **This is the master file.** `CLAUDE.md` and `GEMINI.md` are symlinks to this file.
> Edit ONLY `AGENTS.md` — changes propagate automatically.
> Never delete, overwrite, or replace `CLAUDE.md`/`GEMINI.md` with regular files. If you open one and see only the text `AGENTS.md` inside (one line), it's a broken symlink from a Windows clone — recreate it as a real symlink to `AGENTS.md`, don't treat its contents as instructions.

This project is a focused investigation of a specific topic. The topic is defined at the start of work.

## Structure

- `sources/` — raw materials: transcripts, articles, links, notes
- `instructions/` — tools and methodology
- `results/` — synthesis and conclusions

## Working with Sources

- Each source is one `.md` file at `sources/{slug}.md` — the canonical entry. It carries title, type, URL (if any), references to local files (if any, e.g. `./{slug}.pdf` or `./{slug}.transcript.json`), and user notes. Binary or large content (PDF, audio, full transcripts) lives alongside as separate files referenced from the `.md`.
- Never invent sources. If a source is not in `sources/` and was not shared in the current session — say so.
- When referencing a fact, always cite the source and include a quote:
  > "exact quote from the source" — [title] (file or URL)
- Prefer sources published within the last 2 years. If a source is older, flag it explicitly.
- To determine the current date: check system time via `date` command. Do not rely on training data cutoff.

### Source intake — two flows

**Flow 1: file dropped into `sources/`**
At session start, scan `sources/` for files not yet listed in `_index.md`. Add each one automatically — no user action needed.

Supported formats (one canonical `sources/{slug}.md` per source; binary or transcripts live alongside as referenced files):
- `.md` / `.txt` — already canonical; rename to a sensible slug if needed
- `.pdf` — keep the PDF; create `{slug}.md` referencing it (Claude reads PDFs natively)
- `.mp3` / `.m4a` / `.wav` / other audio — transcribe (see `instructions/tools.md`) into `{slug}.transcript.md`, then create `{slug}.md` referencing both files

**Flow 2: link or content shared in chat**
When the user shares a URL, text, or file in the conversation:
- If it's a YouTube link → create `sources/{slug}.md` with the URL and user notes; fetch the transcript via `instructions/tools.md` into `sources/{slug}.transcript.json` and reference it from the `.md`
- If it's another URL → create `sources/{slug}.md` with the URL and any user notes (no need to pull full content unless asked)
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

For any internet search or fact-check, use the Exa MCP server (`web_search_exa`, `web_fetch_exa`). Never answer from training data when the question is about current state of the world.

Any Python script runs inside a project-local `.venv` — never install dependencies into the system Python. See `instructions/tools.md` for the venv workflow.

## Maintaining This File

**When to add rules:** if the user corrected the same approach twice — that's a signal to add a rule. A one-off correction does not require an update.

**How to add:** one rule = one line. No explanations, no comments — just the rule.

**Size:** target is under 100 lines. At 120+ lines, propose an optimization session to the user before continuing work.

**How to avoid bloat:** if a rule hasn't been recalled in several sessions — it's a candidate for removal. At 120 lines, discuss with the user what to remove or move to `instructions/`.

## Session Start

At the beginning of each session:
1. Read `sources/_index.md` — what has already been processed
2. Read `results/_draft.md` — where we left off
3. If `instructions/methodology.md` has unfilled fields (especially **Topic** and **Why this matters to me**), offer to collect them through a short conversation before deep research begins. Don't auto-fill the motivation — the user's own voice matters here.
