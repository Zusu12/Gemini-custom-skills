# Gemini Custom Developer Skills

This repository contains a collection of custom skills created for the Gemini AI Coding Assistant (**Antigravity**). These skills extend the AI's capabilities, allowing it to automate specific workflows and follow repository-specific best practices.

## Skills Included
### 1. [Research Harvester (Web Scrapper)](./research-harvester)
A skill designed to fetch, extract, and clean web content from multiple public websites based on a user-provided topic or keyword query.
- **Key Features**: Auto-search using Google search, parallel fetching, cleaning HTML to Markdown, error handling/retry logic, and generation of a consolidated Markdown research report.
- **Use Cases**: Gathering research data, monitoring articles on a subject, and saving offline reference documents.

### 2. [Web App Builder](./web-app-builder)
A comprehensive guidelines-and-examples skill for building full-stack web applications using modern web development practices.
- **Key Features**: Standardized component creation guidelines, pre-designed templates, database schemas, and UX/UI design patterns.
- **Use Cases**: Quickly bootstrapping, structuring, and style-polishing web applications.

### 3. [YouTube Notes](./youtube-notes)
A skill that creates structured, high-quality notes from any YouTube video. It extracts video content via transcript API or browser fallback, generates polished Markdown notes, and converts them to PDF after user approval.
- **Key Features**: 7 note styles (Detailed, Short, Summarized, Cornell Method, Mind Map, Bullet Points, Q&A Format), clickable timestamps, auto-generated glossary, flashcards, table of contents, and PDF export via Pandoc or Python fallback.
- **Use Cases**: Taking lecture notes, summarizing tutorials, creating study material, and generating quick-reference documents from video content.

---

## How to Install and Use

To load these skills into your Gemini AI workspace, choose one of the following methods:

### Method 1: Workspace-Level Customization (Recommended)
Copy the skill folder(s) directly into your workspace customization folder:
1. Create a `.agents/skills` directory at the root of your project:
   ```bash
   mkdir -p .agents/skills
   ```
2. Copy the desired skill folder (e.g., `research-harvester` or `web-app-builder`) into `.agents/skills/`.
3. Gemini will automatically discover and load the skill next time you start a conversation in that workspace.

### Method 2: Global Customization
Copy the skill folder(s) into your global Gemini config directory:
- **Windows**: `C:\Users\<Your-Username>\.gemini\config\skills\`
- **Mac/Linux**: `~/.gemini/config/skills/`

---

## Repository Structure

```text
.
├── .gitignore
├── README.md
├── research-harvester/
│   └── SKILL.md
├── web-app-builder/
│   ├── SKILL.md
│   ├── examples/
│   │   ├── components.md
│   │   └── templates.md
│   └── references/
│       └── database-schemas.md
└── youtube-notes/
    └── SKILL.md
```
