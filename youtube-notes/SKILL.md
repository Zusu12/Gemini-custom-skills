---
name: youtube-notes
description: >
  Creates structured notes from YouTube videos in various styles (Detailed,
  Short, Summarized, Cornell Method, Mind Map, Bullet Points, Q&A Format).
  Extracts video content via transcript API or browser fallback, generates a
  polished Markdown file with timestamps, glossary, and table of contents,
  then converts it to PDF after user approval.
  Use when the user wants to take notes from a YouTube video, summarize a
  YouTube lecture, or create study material from video content.
  Do NOT use for non-YouTube video sources or live streams without captions.
---

# YouTube Notes Skill

This skill helps you **create structured, high-quality notes from any YouTube
video**. It extracts the video content, asks the user for their preferred note
style, generates a polished Markdown document, and converts it to PDF after
user review.

---

## When to Use This Skill

Activate this skill when the user says things like:

- "Take notes from this YouTube video"
- "Summarize this YouTube lecture"
- "Create study notes from this video"
- "Make notes from `https://youtube.com/watch?v=...`"
- "I want Cornell notes from this YouTube video"
- "Create flashcards from this video"
- "Generate a mind map from this lecture"

---

## What This Skill Does (Step-by-Step)

### Step 1 — Gather Requirements from the User

Before processing anything, confirm or ask the following:

1. **YouTube URL** — Get the full video URL.
   Accepted formats:
   - `https://www.youtube.com/watch?v=VIDEO_ID`
   - `https://youtu.be/VIDEO_ID`
   - `https://youtube.com/watch?v=VIDEO_ID&t=123`

2. **Note Style** — Ask the user which style they prefer. Present these options
   using the `ask_question` tool:

   | Style | Description |
   |-------|-------------|
   | **Detailed** | Comprehensive, paragraph-based notes covering everything in the video |
   | **Short** | Concise bullet points hitting only the key ideas |
   | **Summarized** | A 1–2 page executive summary of the entire video |
   | **Cornell Method** | Two-column layout: cues/questions on the left, notes on the right, summary at the bottom |
   | **Mind Map** | Hierarchical markdown outline showing topic relationships |
   | **Bullet Points** | Clean, scannable bullet-point format organized by topic |
   | **Q&A Format** | Content restructured as question-and-answer pairs |

3. **Output directory** — Ask the user where they want the notes saved.
   Suggest a sensible default like `./notes/` but always let them specify.

4. **Additional features** — Ask the user which extras they want (use
   `ask_question` with multi-select):
   - ✅ Include timestamps from the video
   - ✅ Auto-extract key terms and create a glossary
   - ✅ Generate flashcards (Q&A pairs) section
   - ✅ Add a table of contents

> **Tip:** If the user is in a hurry or says "just do it", use these defaults:
> Style = **Detailed**, Directory = `./notes/`, All additional features enabled.

---

### Step 2 — Extract Video Content

Use a **two-tier approach** to extract the video content:

#### Tier 1: Transcript API (Preferred — Fast & Reliable)

1. Extract the `VIDEO_ID` from the URL.
2. Use `run_command` to fetch the transcript via a Python script:

```python
# Save this as a scratch script and run it
import json
import urllib.request
import re
import html

def get_transcript(video_id):
    """Fetch YouTube transcript using the video page's captions data."""
    url = f"https://www.youtube.com/watch?v={video_id}"
    req = urllib.request.Request(url, headers={
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
    })
    response = urllib.request.urlopen(req)
    page_html = response.read().decode('utf-8')

    # Extract captions player response
    match = re.search(r'"captions":\s*({.*?"playerCaptionsTracklistRenderer".*?})\s*,\s*"videoDetails"', page_html)
    if not match:
        return None

    captions_json = json.loads(match.group(1))
    tracks = captions_json.get('playerCaptionsTracklistRenderer', {}).get('captionTracks', [])

    if not tracks:
        return None

    # Prefer English, fall back to first available + auto-translate
    caption_url = None
    for track in tracks:
        if track.get('languageCode', '').startswith('en'):
            caption_url = track['baseUrl']
            break
    if not caption_url:
        caption_url = tracks[0]['baseUrl'] + '&tlang=en'

    # Fetch the XML transcript
    caption_url += '&fmt=json3'
    req2 = urllib.request.Request(caption_url, headers={
        'User-Agent': 'Mozilla/5.0'
    })
    resp2 = urllib.request.urlopen(req2)
    data = json.loads(resp2.read().decode('utf-8'))

    segments = []
    for event in data.get('events', []):
        if 'segs' in event:
            text = ''.join(seg.get('utf8', '') for seg in event['segs']).strip()
            if text:
                start_ms = event.get('tStartMs', 0)
                minutes = int(start_ms / 60000)
                seconds = int((start_ms % 60000) / 1000)
                timestamp = f"{minutes:02d}:{seconds:02d}"
                segments.append({'timestamp': timestamp, 'text': text})

    return segments

# Usage
video_id = "VIDEO_ID_HERE"
transcript = get_transcript(video_id)
if transcript:
    print(json.dumps(transcript, indent=2))
else:
    print("NO_TRANSCRIPT_AVAILABLE")
```

> **Important:** Replace `VIDEO_ID_HERE` with the actual video ID before
> running.

#### Tier 2: Browser Fallback (If No Transcript Available)

If Tier 1 returns `NO_TRANSCRIPT_AVAILABLE`, fall back to the browser:

1. Use `browser_subagent` to navigate to the YouTube video page.
2. Instruct the subagent to:
   - Click the "..." (More) menu below the video
   - Click "Show transcript" to open the transcript panel
   - Extract all text from the transcript panel
   - Also extract the video title, channel name, and description
3. If no transcript panel is available, extract content from:
   - The video description
   - Any pinned comments with summaries
   - Chapter markers in the progress bar

> **Note:** If neither tier produces content, inform the user that the video
> has no extractable text content and suggest they provide a video with
> captions enabled.

#### Also Extract Video Metadata

Regardless of which tier succeeds, also extract:
- **Video Title** — from the page title or API
- **Channel Name** — from the channel link
- **Upload Date** — from the video metadata
- **Duration** — from the video player
- **Description** — first 500 characters of the video description

Use `read_url_content` on the YouTube URL or `browser_subagent` for this.

---

### Step 3 — Generate the Markdown Notes

Using the extracted transcript and metadata, generate notes in the user's
chosen style. The Markdown file MUST follow this structure:

```markdown
# 📝 [Video Title]

> **Source:** [YouTube URL](url)
> **Channel:** Channel Name
> **Date:** Upload Date | **Duration:** HH:MM:SS
> **Note Style:** [Chosen Style]
> **Generated:** [Current Date & Time]

---

## 📑 Table of Contents
<!-- Auto-generated TOC linking to all sections below -->

---

## 📖 Notes
<!-- The main notes content in the chosen style goes here -->

---

## 📌 Key Timestamps
<!-- Only if user opted in -->
| Timestamp | Topic |
|-----------|-------|
| [00:00](https://youtube.com/watch?v=ID&t=0) | Introduction |
| [02:15](https://youtube.com/watch?v=ID&t=135) | Main Topic |

---

## 📚 Glossary
<!-- Only if user opted in -->
| Term | Definition |
|------|-----------|
| Term 1 | Brief definition extracted from context |

---

## 🎴 Flashcards
<!-- Only if user opted in -->
### Card 1
**Q:** Question derived from the content
**A:** Answer from the video

### Card 2
**Q:** ...
**A:** ...

---

## 📝 Summary
<!-- A brief 3-5 sentence summary regardless of note style -->

---

*Notes generated by YouTube Notes Skill*
```

#### Style-Specific Formatting Rules

**Detailed:**
- Full paragraphs organized by topic/chapter
- Include all examples, explanations, and nuances mentioned
- Use blockquotes for direct quotes from the speaker
- Target: 80-100% coverage of the video content

**Short:**
- 3-5 bullet points per major topic
- Only the most important ideas
- No examples unless critical
- Target: 20-30% of the video content

**Summarized:**
- Executive summary format: 1-2 pages max
- Opening paragraph with the main thesis/argument
- Body paragraphs covering key points
- Closing paragraph with conclusions/takeaways
- Target: 10-15% of the video content

**Cornell Method:**
Use a markdown table to simulate the Cornell layout:
```markdown
| Cue / Question | Notes |
|----------------|-------|
| What is X? | Detailed notes about X from the video... |
| Why does Y matter? | Explanation of Y's importance... |

### Summary
Brief summary paragraph synthesizing all notes above.
```

**Mind Map:**
```markdown
# Central Topic
  ## Branch 1
    ### Sub-topic 1.1
      - Detail
      - Detail
    ### Sub-topic 1.2
  ## Branch 2
    ### Sub-topic 2.1
```

**Bullet Points:**
```markdown
## Topic 1
- Main point
  - Supporting detail
  - Supporting detail
- Main point

## Topic 2
- Main point
  - Supporting detail
```

**Q&A Format:**
```markdown
### Q: What is the main argument of the video?
**A:** The speaker argues that...

### Q: How does X relate to Y?
**A:** According to the video, X connects to Y through...
```

---

### Step 4 — Present Notes for User Review

1. Save the Markdown file to the user's chosen directory:
   ```
   <output_dir>/<sanitized_video_title>_notes.md
   ```

2. **File naming rules:**
   - Replace spaces with underscores `_`
   - Remove special characters (`/`, `\`, `:`, `*`, `?`, `"`, `<`, `>`, `|`)
   - Keep lowercase and under 60 characters
   - Append `_notes` before the extension

3. Create the file as an **artifact** with:
   - `UserFacing = true`
   - `RequestFeedback = true`
   - A clear summary describing the notes

4. Tell the user:
   - "I've generated your **[Style] notes** for **[Video Title]**."
   - "Please review the notes. Once you approve, I'll generate the PDF."
   - Highlight any sections that may need attention (e.g., unclear segments
     where the transcript was garbled)

5. **Wait for user feedback.** The user may:
   - ✅ Approve as-is → proceed to PDF generation
   - ✏️ Request changes → apply edits and re-present
   - 🔄 Request a different style → regenerate in the new style

---

### Step 5 — Generate PDF from Approved Markdown

Once the user approves the notes, convert the Markdown to PDF.

#### Method 1: Pandoc (Preferred)

Check if Pandoc is installed:
```bash
pandoc --version
```

If available, generate PDF:
```bash
pandoc "<input_file>.md" -o "<output_file>.pdf" \
  --pdf-engine=xelatex \
  -V geometry:margin=1in \
  -V fontsize=11pt \
  -V mainfont="Segoe UI" \
  --toc \
  --highlight-style=tango \
  -V colorlinks=true \
  -V linkcolor=blue \
  -V urlcolor=blue \
  --metadata title="Video Notes"
```

If `xelatex` is not available, try:
```bash
pandoc "<input_file>.md" -o "<output_file>.pdf" \
  --pdf-engine=pdflatex \
  -V geometry:margin=1in \
  -V fontsize=11pt \
  --toc
```

#### Method 2: Python Fallback (If Pandoc Not Installed)

Use a Python script with `markdown2` and `pdfkit` (or `weasyprint`):

```python
# Scratch script for PDF generation
import subprocess
import sys

# Install dependencies
subprocess.check_call([sys.executable, '-m', 'pip', 'install',
                       'markdown2', 'weasyprint'])

import markdown2
from weasyprint import HTML

def md_to_pdf(md_path, pdf_path):
    with open(md_path, 'r', encoding='utf-8') as f:
        md_content = f.read()

    html_content = markdown2.markdown(
        md_content,
        extras=['tables', 'fenced-code-blocks', 'header-ids',
                'toc', 'strike', 'task_list']
    )

    styled_html = f"""
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8">
        <style>
            body {{
                font-family: 'Segoe UI', Arial, sans-serif;
                line-height: 1.6;
                max-width: 800px;
                margin: 40px auto;
                padding: 0 20px;
                color: #333;
            }}
            h1 {{ color: #1a1a2e; border-bottom: 2px solid #e94560; padding-bottom: 10px; }}
            h2 {{ color: #16213e; border-bottom: 1px solid #ddd; padding-bottom: 5px; }}
            h3 {{ color: #0f3460; }}
            blockquote {{
                border-left: 4px solid #e94560;
                margin: 1em 0;
                padding: 0.5em 1em;
                background: #f8f9fa;
            }}
            table {{
                border-collapse: collapse;
                width: 100%;
                margin: 1em 0;
            }}
            th, td {{
                border: 1px solid #ddd;
                padding: 8px 12px;
                text-align: left;
            }}
            th {{ background: #16213e; color: white; }}
            tr:nth-child(even) {{ background: #f8f9fa; }}
            code {{
                background: #f4f4f4;
                padding: 2px 6px;
                border-radius: 3px;
                font-size: 0.9em;
            }}
            a {{ color: #e94560; }}
        </style>
    </head>
    <body>
        {html_content}
    </body>
    </html>
    """

    HTML(string=styled_html).write_pdf(pdf_path)
    print(f"PDF saved to: {pdf_path}")

md_to_pdf("INPUT_PATH", "OUTPUT_PATH")
```

> **Important:** Replace `INPUT_PATH` and `OUTPUT_PATH` with actual file paths
> before running. Ensure the virtual environment has the required packages.

#### Method 3: Browser-Based Fallback (Last Resort)

If neither Pandoc nor Python PDF libraries work:

1. Use `browser_subagent` to open the Markdown file rendered as HTML
2. Use the browser's print-to-PDF functionality
3. Save the resulting PDF to the output directory

#### After PDF Generation

1. Confirm the PDF was created successfully
2. Tell the user: "✅ PDF generated at `<path/to/file.pdf>`"
3. Offer to open the file or make any final adjustments

---

## Output Structure

After the skill runs, the user's output directory will contain:

```
<output_dir>/
├── video_title_notes.md     ← The approved Markdown notes
└── video_title_notes.pdf    ← The generated PDF
```

---

## Tools Used by This Skill

| Tool | Purpose |
|------|---------|
| `ask_question` | Gather note style preference and feature selections from the user |
| `search_web` | Look up video metadata if needed |
| `read_url_content` | Fetch YouTube page content and transcript data |
| `browser_subagent` | Fallback: extract transcript from YouTube's transcript panel |
| `run_command` | Execute Python scripts for transcript extraction and PDF generation |
| `write_to_file` | Save the generated Markdown notes and any scratch scripts |
| `view_file` | Preview generated files |

---

## Limitations & Important Notes

- ❌ **No live streams** — Live streams without saved captions cannot be processed.
- ❌ **No audio-only extraction** — This skill does not perform speech-to-text.
  It relies on existing captions/transcripts.
- ❌ **Age-restricted videos** — Videos requiring sign-in may not be accessible
  via the transcript API. Browser fallback may work if the user is logged in.
- ❌ **Auto-generated captions quality** — YouTube's auto-generated captions can
  contain errors. The skill will do its best to clean up obvious issues, but
  some garbled text may remain. Flag these sections for the user.
- ✅ **Multiple languages** — If the video is in a non-English language with
  English captions available, those will be used. Otherwise, auto-translation
  is attempted.
- ✅ **Long videos supported** — Videos of any length are supported, though very
  long videos (2+ hours) may produce extensive notes.
- ✅ **Chapter markers** — If the video has chapter markers, use them as natural
  section dividers in the notes.

---

## Example Workflow

**User says:** "Take notes from https://youtube.com/watch?v=dQw4w9WgXcQ"

1. **Ask preferences** using `ask_question`:
   - Note style? → User picks "Cornell Method"
   - Output directory? → User says `D:\StudyNotes\`
   - Additional features? → User selects: Timestamps, Glossary, TOC

2. **Extract transcript** via Tier 1 (Python transcript fetcher)
   - Successfully extracts 847 transcript segments with timestamps

3. **Extract metadata** via `read_url_content`
   - Title: "Never Gonna Give You Up - Rick Astley"
   - Channel: "Rick Astley"
   - Duration: 3:33

4. **Generate Markdown** in Cornell Method format
   - Save to: `D:\StudyNotes\never_gonna_give_you_up_notes.md`
   - Includes: TOC, Cornell table, Timestamps section, Glossary, Summary

5. **Present to user** as an artifact with `RequestFeedback = true`
   - User reviews and says "Looks good, generate the PDF"

6. **Generate PDF** using Pandoc
   - Save to: `D:\StudyNotes\never_gonna_give_you_up_notes.pdf`
   - Tell user: "✅ PDF saved to `D:\StudyNotes\never_gonna_give_you_up_notes.pdf`"

---

## Error Handling

| Scenario | Action |
|----------|--------|
| Invalid YouTube URL | Ask the user to provide a valid YouTube URL |
| No transcript available (both tiers fail) | Inform user, suggest a video with captions |
| Pandoc not installed | Fall back to Python PDF generation |
| Python PDF libraries fail to install | Fall back to browser-based PDF |
| All PDF methods fail | Deliver the Markdown file and explain the user can convert manually |
| Transcript is garbled/low quality | Flag problematic sections with `<!-- unclear -->` comments |
| Video is very long (2+ hours) | Warn user that notes will be extensive, offer to do Summarized style instead |
