# Vibe Research Template

A minimal template for AI-assisted research on a specific topic.

## Structure

```
CLAUDE.md                    # Claude instructions
sources/
  _index.md                  # Source tracker
instructions/
  tools.md                   # Available tools
  methodology.md             # Research methodology template
results/
  _draft.md                  # Working synthesis
```

## How to Use

1. **Fork or copy this repository** for your research topic
2. **Choose your language** — all template files are in English by default
3. **Tell Claude Code your language preference** at the start of the first session

   Claude will generate localized versions of `CLAUDE.md`, `instructions/`, and `results/` files in your chosen language, leaving the template originals as reference.

4. **Define the topic** — fill in `instructions/methodology.md`
5. **Add sources** — register each one in `sources/_index.md` before using it
6. **Synthesize** — build up `results/_draft.md` as you go

## YouTube Sources

Use the tool described in `instructions/tools.md` to extract transcripts from YouTube videos before analysis.

## Tips

- Keep `CLAUDE.md` under 100 lines — it's a living document, not a manual
- One research topic per repository
- Commit regularly so you can trace how conclusions evolved
