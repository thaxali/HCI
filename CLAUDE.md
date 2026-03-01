# Claude Code Instructions — HCI Reading List

## What This Repo Is
A personal reading list and synthesis tool. The owner sends you a URL to an article (arxiv paper, blog post, newsletter, etc.) and you:
1. Read the article
2. Synthesize it into a TTS-optimized analysis
3. Add it to the reading list HTML
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
2. **Determine the next paper ID** — look at the `papers` array in `CHI26/index.html` and find the highest existing `id`, then use `id + 1`
3. **Write the analysis** as a template literal `const paper{N}Analysis = \`...\`;` — insert it right before the `// ── Papers Data ──` comment
4. **Add the paper to the `papers` array** at the end (before the closing `];`)
5. **Update the `analysisMap`** object to include the new paper ID and analysis variable
6. **Commit and push** to `main`

## File Structure

```
index.html              ← Password gate (entry point for GitHub Pages)
queue.md                ← URL queue — add URLs here to trigger processing
CHI26/
  index.html            ← THE reading list (single-file HTML app, ~215KB)
  notes/                ← Exported reading notes (markdown)
  notes.md              ← Notes stub
CHI26_Curated_Papers.md ← Reference list of CHI'26 preprints
README.md               ← Project overview
.github/workflows/
  process-queue.yml     ← GitHub Action that triggers Claude Code on queue changes
```

The only file you need to edit is **`CHI26/index.html`** (and `queue.md` when processing the queue).

## How to Edit `CHI26/index.html`

### Step 1: Add the analysis variable

Find the line `// ── State ──` (around line 2641). Insert your new analysis BEFORE that line:

```js
const paper{N}Analysis = `
<div class="analysis-panel open">
  <!-- analysis HTML here -->
</div>
`;

// ── State ──
```

### Step 2: Add to the papers array

Find the `const papers = [` array. Add your new entry at the end:

```js
  {
    id: {N},
    title: "Full Article Title",
    relevance: "One-sentence <em>relevance description</em> with key phrase emphasized.",
    pdf: "https://...",       // direct PDF link if available, otherwise article URL
    arxiv: "https://...",     // arxiv abs link if applicable, otherwise omit
    tags: ["tag1", "tag2"],   // see Tag Reference below
    read: false,
    hasAnalysis: true
  }
```

### Step 3: Update the analysisMap

Find the `const analysisMap = {` line and add the new entry:

```js
const analysisMap = { 1: paper1Analysis, ..., {N}: paper{N}Analysis };
```

## Analysis HTML Template

Every analysis must follow this structure. Write in **conversational prose** optimized for text-to-speech (~12-15 min listen). No bullet points in the body — use flowing paragraphs.

```html
<div class="analysis-panel open">
  <div class="analysis-header">
    <div class="analysis-badge">HCI Paper Analysis · TTS Optimized</div>
    <div class="analysis-meta">~{X} min listen</div>
  </div>

  <div class="tts-controls">
    <button class="tts-btn" onclick="ttsPlayAll()">▶ Play All</button>
    <button class="tts-btn" onclick="ttsPause()">⏸ Pause</button>
    <button class="tts-btn" onclick="ttsStop()">⏹ Stop</button>
    <span class="tts-status">Click any paragraph to start</span>
  </div>

  <div class="tts-vitals">
    <strong>Title:</strong> {title}<br>
    <strong>Authors:</strong> {authors}<br>
    <strong>Source:</strong> {venue/publication/blog}<br>
    <strong>One-liner:</strong> {single sentence summary}
  </div>

  <div class="tts-section">
    <div class="tts-section-title">TL;DR — Why You Should Care</div>
    <p class="tts-paragraph" onclick="ttsSpeak(this)">...</p>
    <p class="tts-paragraph" onclick="ttsSpeak(this)">...</p>
  </div>

  <div class="divider"></div>

  <div class="tts-section">
    <div class="tts-section-title">The Core Contribution</div>
    <p class="tts-paragraph" onclick="ttsSpeak(this)">...</p>
  </div>

  <div class="divider"></div>

  <div class="tts-section">
    <div class="tts-section-title">Key Insights and Evaluation</div>
    <p class="tts-paragraph" onclick="ttsSpeak(this)">...</p>
    <!-- Use <span class="highlight">...</span> for key insights -->
    <!-- Use <span class="stat-highlight">...</span> for statistics -->
  </div>

  <div class="divider"></div>

  <div class="tts-section">
    <div class="tts-section-title">Seena Labs Relevance</div>
    <p class="tts-paragraph" onclick="ttsSpeak(this)">...</p>
    <!-- This is the MOST IMPORTANT section — be specific about implications -->
    <!-- If not relevant to Seena, rename to "Why This Matters" -->
  </div>

  <div class="divider"></div>

  <div class="tts-section">
    <div class="tts-section-title">Everything is Designed — Social Media Angle</div>
    <p class="tts-paragraph" onclick="ttsSpeak(this)">...</p>
    <!-- Write the "dinner party version" — how you'd explain this over drinks -->
  </div>
</div>
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

## Writing Style

- Conversational, second-person ("you") prose — like explaining to a smart friend
- Optimized for TTS listening — spell out numbers, avoid acronym soup
- No bullet points in analysis body
- Use `<span class="highlight">key insight here</span>` for important ideas
- Use `<span class="stat-highlight">42% improvement</span>` for citable numbers
- Every `<p>` gets `class="tts-paragraph" onclick="ttsSpeak(this)"`
- Escape backticks in the analysis text since it lives inside a JS template literal (use `\``)

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
