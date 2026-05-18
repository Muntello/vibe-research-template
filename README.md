# Vibe Research Template

A minimal template for AI-assisted research on a specific topic using Claude Code.

## Quick Start

Open Claude Code in an empty directory and paste this prompt:

```
Bootstrap a research project in the current directory using this template:
https://github.com/Muntello/vibe-research-template

Do this:
1. Clone the template into a temporary subfolder.
2. Move all files (including hidden ones: .claude/, .mcp.json, .gitignore, .env.example) into the current directory. Skip the template's .git folder.
3. Delete the temporary subfolder.
4. Rename `.claude/settings.json` to `.claude/settings.local.json` so the pre-approved permissions stay local.
5. Verify the layout exists: AGENTS.md (plus CLAUDE.md and GEMINI.md as symlinks to it), README.md, instructions/tools.md, instructions/methodology.md, sources/_index.md, results/_draft.md, .claude/settings.local.json, .mcp.json, .gitignore.

Then ask me what topic I want to research and what made me start thinking about it.
```

Prefer to set it up by hand? See **How to Use** below.

## Structure

```
.env.example                 # API key placeholders (copy to .env, never commit)
.mcp.json                    # MCP servers (Exa for internet search)
.claude/
  settings.json              # Pre-approved MCP + safe agent permissions
CLAUDE.md                    # Claude instructions — living document, keep under 100 lines
instructions/
  tools.md                   # Available tools and how to call them
  methodology.md             # Research scope template — fill in at topic start
sources/
  _index.md                  # Source tracker — maintained by Claude, not manually
results/
  _draft.md                  # Working synthesis
```

## How to Use

1. **Fork or copy this repository** for your research topic — one repo per topic
2. **Choose your language** — all template files are in English by default.
   Tell Claude Code your preference at the start of the first session and it will
   generate localized versions of all files.
3. **Set up API keys** (optional) — copy `.env.example` to `.env` and fill in keys
   if you plan to transcribe audio files. `.env` is gitignored and stays local.
   Internet search via Exa MCP (`.mcp.json`) works out of the box — no key needed.
4. **Define the topic** — open `instructions/methodology.md` and fill it in,
   or just tell Claude Code what you're researching and it will help you fill it
   through a short conversation (especially the "why this matters to me" part).
5. **Add sources** — Claude handles the index automatically. Just:
   - drop files into `sources/`
   - or share links and content directly in the chat
6. **Synthesize** — Claude builds up `results/_draft.md` as sources are processed.
   Final documents go in `results/` with descriptive names.

## Supported Source Types

| Type | How to add |
|------|------------|
| YouTube video | Paste link in chat — Claude fetches the transcript |
| Article / webpage | Paste link in chat — Claude creates `sources/{slug}.md` with the URL and your notes |
| PDF | Drop into `sources/` — Claude extracts text automatically |
| Text / notes | Paste in chat — Claude saves to `sources/` |
| Audio file | Convert to text first (see `instructions/tools.md`), then drop into `sources/` |

## Tips

- `CLAUDE.md` is a living document — Claude updates it when you correct an approach twice.
  If it grows past 120 lines, Claude will suggest an optimisation session.
- Commit regularly so you can trace how conclusions evolved.
- Source notes are saved as `{source-name}.notes.md` alongside each source —
  key quotes, relevance to the topic, ideas to explore further.
