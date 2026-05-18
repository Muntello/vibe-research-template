# Research Tools

## Internet Search — Exa MCP

**Configured in:** `.mcp.json` (project root)
**Server:** `https://mcp.exa.ai/mcp` — no API key required for default use.

If you hit rate limits on intensive research, grab a free key at
https://dashboard.exa.ai/api-keys and add it as a header in `.mcp.json`:
`"headers": { "x-api-key": "YOUR_KEY" }`.

### When to use

Use Exa for any task that requires fresh information from the web:
- verifying a factual claim
- finding sources for the current research topic
- looking up recent events, papers, announcements, or releases
- following up on a name, term, or product mentioned in a source

Never answer from training data when the question is about current state of the
world. If you have not seen the source in `sources/` and have not searched the
web in this session — search first.

### Available tools

| Tool | Purpose |
|------|---------|
| `web_search_exa` | General web search, returns cleaned content snippets |
| `web_fetch_exa` | Retrieve a single URL's full content as markdown |

### Workflow after searching

1. If a result is worth keeping, save it as a source. Every source is one canonical `sources/{slug}.md`:
   - Bookmark only → create `sources/{slug}.md` with the URL and a short user note
   - Full page worth quoting → fetch with `web_fetch_exa`, put the content inline in the same `.md` under `## Content` (or in a sibling `{slug}.content.md` if large) and reference it
2. Register the source in `sources/_index.md`.
3. Always cite with the exact URL when referencing a fact from search results.
4. Flag any source older than 2 years explicitly.

---

## YouTube Transcript Extractor

**URL:** https://youtubetranscribe.khabaroff.studio  
**No API key required.**

Accepts any YouTube URL format: `youtube.com/watch?v=`, `youtu.be/`, `/shorts/`, `/live/`, or just the 11-character video ID.

### When to use

Any time the user provides a YouTube link as a source. Always fetch the transcript before analysis — never summarize from memory of a video.

### Endpoints

| Format | Endpoint | Best for |
|--------|----------|----------|
| JSON | `/transcript/{video_id}` | **Primary format** — includes timestamps, title, language |
| TXT | `/transcript/{video_id}/txt` | Quick read, pasting into prompts without structure |
| VTT | `/transcript/{video_id}/vtt` | Subtitle-style output with timing |

### Claude Code usage

The transcript is a **sibling file** to the canonical `sources/{slug}.md` wrapper. First make sure the wrapper exists (with the YouTube URL and a brief user note), then fetch the transcript next to it.

**Fetch JSON with timestamps (primary workflow):**
```bash
curl -s "https://youtubetranscribe.khabaroff.studio/transcript/dQw4w9WgXcQ" \
  > sources/video-title.transcript.json
```

**Extract video ID from a full URL and fetch:**
```bash
VIDEO_ID=$(echo "https://www.youtube.com/watch?v=dQw4w9WgXcQ" | grep -oP '(?<=v=)[^&]+')
curl -s "https://youtubetranscribe.khabaroff.studio/transcript/${VIDEO_ID}" \
  > sources/video-title.transcript.json
```

**Shorts:**
```bash
VIDEO_ID=$(echo "https://www.youtube.com/shorts/dQw4w9WgXcQ" | grep -oP '[^/]+$')
curl -s "https://youtubetranscribe.khabaroff.studio/transcript/${VIDEO_ID}" \
  > sources/video-title.transcript.json
```

### Analysing a video for relevance

When the user asks "what parts of this video are worth watching?", fetch JSON and ask:

> Read this transcript JSON. For each segment relevant to [topic], extract:
> - the key idea in one sentence
> - a direct link to that moment: `https://youtube.com/watch?v={video_id}&t={start_seconds}`
>
> Group by theme. Skip irrelevant segments entirely.

YouTube timestamp links use seconds: `&t=183` jumps to 3:03 in the video.

### After fetching

1. Confirm `sources/{slug}.md` (the canonical wrapper) exists with the YouTube URL, a short user note, and a reference to the transcript file (e.g. `sources/2024-andrej-karpathy-llm-intro.md` referencing `2024-andrej-karpathy-llm-intro.transcript.json`).
2. Add an entry to `sources/_index.md` with status `done`.

---

## Audio Transcription

Audio files (`.mp3`, `.m4a`, `.wav`, etc.) must be converted to text before they can be used as sources.

**General approach:** transcribe the audio with any tool you have available; save the transcript as `sources/{slug}.transcript.md` (a sibling file), then create the canonical `sources/{slug}.md` wrapper referencing both the audio file and the transcript.

**Options:**
- Any online transcription service (upload file, copy text, save as `.md`)
- macOS Sonoma+ has built-in transcription in Voice Memos — export as text
- Phone apps: Otter.ai, Whisper transcription apps

### Advanced: API-based transcription (single curl command)

If you have an API key, these work on any platform with no installation.

**Groq (free tier):**
1. Get a key at https://console.groq.com/keys
2. Copy `.env.example` to `.env` and set `GROQ_API_KEY=your_key_here`
3. Run:
```bash
source .env
curl -s https://api.groq.com/openai/v1/audio/transcriptions \
  -H "Authorization: Bearer $GROQ_API_KEY" \
  -F "file=@sources/recording.mp3" \
  -F "model=whisper-large-v3" \
  -F "response_format=text" \
  > sources/recording.transcript.md
```

**OpenAI ($0.006/min):**
```bash
curl -s https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F "file=@sources/recording.mp3" \
  -F "model=whisper-1" \
  -F "response_format=text" \
  > sources/recording.transcript.md
```

---

## PDF Sources

No extra tools needed — Claude Code reads PDFs natively. Drop the file into `sources/` and Claude will extract the text automatically.
