# Claude Code Instructions — HCI Reading List

## What This Repo Is
A personal reading list and synthesis tool. The owner sends you a URL to an article (arxiv paper, blog post, newsletter, etc.) and you:
1. Read the article
2. Synthesize it into a TTS-optimized analysis (Markdown)
3. Add the paper metadata to `papers.json` and create a `.md` analysis file
4. Commit and push

## Two Workflows

### Workflow 1: Queue (automated via GitHub Action)
The owner adds URLs to `queue.md` under `## Pending` (one per line). A GitHub Action detects the change and runs Claude Code to process them.

When processing the queue:
1. Read `queue.md` and find all URLs under `## Pending`
2. For each URL, follow the steps in Workflow 2 below
3. After each article is successfully added, move its URL from `## Pending` to `## Processed` in `queue.md`, prefixed with the paper number (e.g., `- #27 https://example.com`)
4. If a URL cannot be fetched, leave it in Pending with a note: `- https://... (fetch failed — retry later)`
5. Commit after each article

### Workflow 2: Direct (user gives you a URL)
When the user gives you a URL directly:

1. **Fetch and read the article** using WebFetch or by downloading the PDF
2. **Determine the next paper ID** — look at `CHI26/papers.json` and find the highest existing `id`, then use `id + 1`
3. **Add the paper to `papers.json`** — append a new entry to the JSON array
4. **Write the analysis as a Markdown file** — create `CHI26/papers/{NNN}.md` (zero-padded to 3 digits, e.g. `027.md`)
5. **Commit and push** to `main`

## File Structure

```
index.html              ← Password gate (entry point for GitHub Pages)
queue.md                ← URL queue — add URLs here to trigger processing
CHI26/
  index.html            ← Reading list UI (static shell, loads data dynamically)
  papers.json           ← Paper metadata array (title, tags, URLs, etc.)
  papers/               ← Individual analysis files (one .md per paper)
    001.md
    002.md
    ...
    026.md
  notes/                ← Exported reading notes (markdown)
  notes.md              ← Notes stub
CHI26_Curated_Papers.md ← Reference list of CHI'26 preprints
README.md               ← Project overview
CLAUDE.md               ← These instructions
.github/workflows/
  process-queue.yml     ← GitHub Action that triggers Claude Code on queue changes
```

The files you edit are:
- **`CHI26/papers.json`** — add new paper metadata
- **`CHI26/papers/{NNN}.md`** — create new analysis markdown file
- **`queue.md`** — when processing the queue (move URLs from Pending to Processed)

You do NOT need to edit `CHI26/index.html` — it loads data dynamically.

## How to Add a Paper

### Step 1: Add to papers.json

Open `CHI26/papers.json` and add a new entry at the end of the array:

```json
{
  "id": 27,
  "title": "Full Article Title",
  "relevance": "One-sentence <em>relevance description</em> with key phrase emphasized.",
  "pdf": "https://...",
  "arxiv": "https://...",
  "tags": ["tag1", "tag2"],
  "hasAnalysis": true
}
```

Fields:
- `id` — next sequential integer
- `title` — full article title
- `relevance` — one sentence, use `<em>` for key phrases
- `pdf` — direct PDF link or article URL
- `arxiv` — arxiv abs link (omit if not applicable)
- `tags` — array of tag strings (see Tag Reference below)
- `hasAnalysis` — `true` if you're writing an analysis file
- `note` — (optional) editorial note
- `essential` — (optional) `true` to flag as essential reading

### Step 2: Write the analysis Markdown file

Create `CHI26/papers/{NNN}.md` where NNN is the zero-padded paper ID (e.g., `027.md`).

## Analysis Markdown Template

Every analysis must follow this structure. Write in **conversational prose** optimized for text-to-speech (~12-15 min listen). No bullet points in the body — use flowing paragraphs.

```markdown
*HCI Paper Analysis · TTS Optimized · ~{X} min listen*

**Title:** {title}
**Authors:** {authors}
**Source:** {venue/publication/blog}
**One-liner:** {single sentence summary}

---

## TL;DR — Why You Should Care

Here's the core insight... (2-3 paragraphs, hook + summary)

---

## The Core Contribution

This paper makes... (methodology, contribution type, strength rating)

---

## Key Insights and Evaluation

The strongest aspect... Use **bold** for key insights.

---

## Seena Labs Relevance

This is directly relevant... (MOST IMPORTANT section — be specific)
If not relevant to Seena, rename to "## Why This Matters"

---

## Everything is Designed — Social Media Angle

Here's the dinner party version... (how you'd explain this over drinks)
```

### Section Guide

For **academic papers**, include all of these:
1. **TL;DR — Why You Should Care** — 2-3 paragraphs, hook + summary
2. **The Core Contribution** — Methodology, contribution type, strength rating
3. **Paper Evaluation** — Strengths and weaknesses
4. **Similar Reading** — Key references
5. **Seena Labs Relevance** — Specific implications (longest section)
6. **Empirical Evidence Worth Citing** — Stats for pitches/writing
7. **Everything is Designed — Social Media Angle** — Content hook
8. **Industry vs. Theory** — Practical applicability

For **blog posts / articles / non-academic content**, use a lighter structure:
1. **TL;DR — Why You Should Care**
2. **The Core Argument**
3. **Key Insights and Evaluation**
4. **Seena Labs Relevance** (or **Why This Matters** if not Seena-specific)
5. **Everything is Designed — Social Media Angle**

For the first line (badge), use:
- `*HCI Paper Analysis · TTS Optimized · ~{X} min listen*` for academic papers
- `*Article Analysis · TTS Optimized · ~{X} min listen*` for blog posts/articles

## Writing Style

- Conversational, second-person ("you") prose — like explaining to a smart friend
- Optimized for TTS listening — spell out numbers, avoid acronym soup
- No bullet points in analysis body — use flowing paragraphs
- Use `**bold text**` for key insights and important ideas
- Use `---` between sections as dividers
- Use `## Section Title` for section headers
- Every paragraph should be its own block (separated by blank lines)

## Tag Reference

| Tag | Use for |
|-----|---------|
| `seena` | Core Seena Labs platform relevance |
| `brainspace` | Brain Space product relevance |
| `eid` | "Everything is Designed" content angle |
| `interviews` | Interview methodology, micro-interviews |
| `behavior` | Behavioral signals, pattern detection |
| `agents` | AI agents, orchestration |
| `qual` | Qualitative research methods |
| `bias` | LLM bias, sycophancy |
| `valuesensitive` | Value-sensitive design |

You can create new tags if needed — they'll render automatically.

## Git Workflow

- Always commit and push to `main` after adding an article
- Commit message format: `Add paper #{N}: {Short Title}`
- One article per commit
