---
name: research-harvester
description: >
  Fetches and saves web content from multiple public sites based on a
  user-provided topic or keyword. Use this skill when the user wants to
  collect raw HTML or plain text from several URLs related to a research
  topic and save the results to local files for later review.
  Do NOT use this skill if authentication (API keys, login, tokens) is required.
---

# Research Harvester Skill

This skill helps you **collect web content from multiple public websites** based
on a topic or keyword query. The results are saved as raw HTML or plain text
files in a local output directory.

---

## When to Use This Skill

Activate this skill when the user says things like:

- "Fetch articles about X and save them"
- "Scrape multiple sites about Y"
- "Collect web content on topic Z"
- "Download pages related to [keyword]"

---

## What This Skill Does (Step-by-Step)

### Step 1 — Understand the Request

Before fetching anything, confirm the following with the user (if not already clear):

1. **Topic or keyword** — What are we searching for? (e.g., "climate change 2024")
2. **Number of sources** — How many sites/pages should be fetched? (default: 3–5)
3. **Output format** — Save as plain text (`.txt`) or raw HTML (`.html`)?
   Prefer `.txt` for readability unless the user needs the full markup.
4. **Output folder** — Where should files be saved?
   Default: `./harvested/` inside the current workspace.

> **Tip:** If the user is vague, make reasonable assumptions and clearly state
> them before proceeding.

---

### Step 2 — Find Target URLs

Use the `search_web` tool to find relevant public URLs for the topic/keyword.

- Search query example: `"climate change 2024" research articles`
- Pick the **top 3–5 results** that look like real content pages
  (avoid login walls, paywalls, or redirect-only pages).
- Record the list of URLs you plan to fetch.

> **Note:** Only fetch from publicly accessible pages — no authentication is
> needed or supported by this skill.

---

### Step 3 — Fetch Each URL

Use the `read_url_content` tool to fetch the content of each URL.

- For each URL:
  - Call `read_url_content` with the URL.
  - The tool returns markdown-converted content (HTML → markdown).
  - If a URL fails (timeout, 404, redirect), skip it and note the failure.
- Keep track of:
  - ✅ Successfully fetched URLs
  - ❌ Failed URLs (with a short reason)

> **Tip:** Fetch one URL at a time. Do not batch all at once — some sites may
> be slow or fail, and it's easier to handle errors individually.

---

### Step 4 — Save Content to Files

For each successfully fetched page, save the content to a local file.

**File naming convention:**
```
./harvested/<sanitized-topic>_<index>.<ext>
```

Example for topic "climate change 2024", index 1, plain text format:
```
./harvested/climate_change_2024_1.txt
```

**Sanitize the filename:**
- Replace spaces with underscores `_`
- Remove special characters (`/`, `\`, `:`, `*`, `?`, `"`, `<`, `>`, `|`)
- Keep it lowercase and under 60 characters

Use the `write_to_file` tool to create each file.

> **Tip:** Always create the `./harvested/` folder first if it doesn't exist.
> You can do this by writing any file into it — the tool creates parent
> directories automatically.

---

### Step 5 — Generate a Summary Report

After fetching and saving all files, create a summary report:

**File:** `./harvested/SUMMARY.md`

**Contents of the report:**

```markdown
# Research Harvest Report

**Topic:** <user's topic/keyword>
**Date:** <current date>
**Total URLs attempted:** <N>
**Successfully saved:** <N>
**Failed:** <N>

## Saved Files

| # | Source URL | Saved File | Status |
|---|-----------|------------|--------|
| 1 | https://... | harvested/topic_1.txt | ✅ Success |
| 2 | https://... | N/A | ❌ Failed (404) |

## Notes

- List any important observations about the content quality.
- Mention if some pages had very little content (< 200 words).
```

---

## Output Structure

After the skill runs, the workspace will look like:

```
./harvested/
├── SUMMARY.md             ← Overview of all fetched content
├── topic_keyword_1.txt    ← Content from source 1
├── topic_keyword_2.txt    ← Content from source 2
└── topic_keyword_3.txt    ← Content from source 3
```

---

## Tools Used by This Skill

| Tool | Purpose |
|------|---------|
| `search_web` | Find relevant public URLs for the topic |
| `read_url_content` | Fetch and convert page content to markdown/text |
| `write_to_file` | Save fetched content and the summary report |

---

## Limitations & Important Notes

- ❌ **No authentication** — this skill only works with publicly accessible pages.
- ❌ **No JavaScript rendering** — pages that require JS to load content
  (e.g., React SPAs) may return empty or partial results. Use `browser_subagent`
  instead if JS rendering is needed.
- ❌ **No rate limiting** — avoid fetching too many pages from the same domain
  in a short time to be a good citizen of the web.
- ✅ **Retry once** — if a fetch fails, retry the URL once before marking it
  as failed.

---

## Example Workflow

**User says:** "Harvest 3 articles about quantum computing breakthroughs"

1. Search: `quantum computing breakthroughs research 2024`
2. Pick top 3 URLs from results
3. Fetch each URL with `read_url_content`
4. Save to:
   - `./harvested/quantum_computing_1.txt`
   - `./harvested/quantum_computing_2.txt`
   - `./harvested/quantum_computing_3.txt`
5. Write `./harvested/SUMMARY.md` with a report table
6. Tell the user: "Saved 3 articles to `./harvested/`. See `SUMMARY.md` for details."
