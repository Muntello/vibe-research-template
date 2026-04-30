# Research Tools

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

**Fetch JSON with timestamps and save to sources/ (primary workflow):**
```bash
curl -s "https://youtubetranscribe.khabaroff.studio/transcript/dQw4w9WgXcQ" \
  > sources/video-title.json
```

**Extract video ID from a full URL and fetch:**
```bash
VIDEO_ID=$(echo "https://www.youtube.com/watch?v=dQw4w9WgXcQ" | grep -oP '(?<=v=)[^&]+')
curl -s "https://youtubetranscribe.khabaroff.studio/transcript/${VIDEO_ID}" \
  > sources/video-title.json
```

**Shorts:**
```bash
VIDEO_ID=$(echo "https://www.youtube.com/shorts/dQw4w9WgXcQ" | grep -oP '[^/]+$')
curl -s "https://youtubetranscribe.khabaroff.studio/transcript/${VIDEO_ID}" \
  > sources/video-title.json
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

1. Save file to `sources/` with a descriptive name (e.g. `sources/2024-andrej-karpathy-llm-intro.md`)
2. Add an entry to `sources/_index.md` with status `done`

---

## Audio Transcription

Audio files (`.mp3`, `.m4a`, `.wav`, etc.) must be converted to text before they can be used as sources.

**General approach:** use any transcription tool you have available, save the result as `{name}.md` in `sources/`, then drop it into the conversation or let Claude detect it at session start.

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
  > sources/recording.md
```

**OpenAI ($0.006/min):**
```bash
curl -s https://api.openai.com/v1/audio/transcriptions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F "file=@sources/recording.mp3" \
  -F "model=whisper-1" \
  -F "response_format=text" \
  > sources/recording.md
```

---

## PDF Sources

No extra tools needed — Claude Code reads PDFs natively. Drop the file into `sources/` and Claude will extract the text automatically.
