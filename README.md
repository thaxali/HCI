# CHI'26 Reading List — Continuation Brief

## What This Is
An interactive HTML reading list of 25 CHI 2026 HCI+AI preprints relevant to Seena Labs and Brain Space, with embedded paper analyses optimized for TTS (text-to-speech) listening. The artifact is styled with Seena's design system and lives at:

- **GitHub repo:** https://github.com/thaxali/HCI.git
- **GitHub Pages:** https://thaxali.github.io/HCI (once Pages is enabled, file is `index.html`)
- **Entry point:** `index.html` (password gate) → `CHI26/index.html` (reading list)

## Current Status — 8 of 25 papers analyzed

### ✅ Completed Analyses
| # | Paper | arxiv | Tags |
|---|-------|-------|------|
| 1 | **Sensing What Surveys Miss** | 2602.00880 | interviews, behavior |
| 2 | **InterFlow** | 2602.06396 | interviews |
| 3 | **Behavioral Indicators of Overreliance** | 2602.11567 | behavior, agents |
| 4 | **Qualitative Coding Analysis through Open-Source LLMs** | 2602.18352 | qual |
| 5 | **Reflexis** | 2601.15445 | qual |
| 6 | **When Should Users Check?** | 2510.05307 | agents, behavior |
| 7 | **Interaction Context Often Increases Sycophancy in LLMs** | 2509.12517 | bias, interviews |
| 8 | **Designing Computational Tools for Exploring Causal Relationships in Qualitative Data** | 2602.06506 | qual, behavior |

### ⬜ Remaining Papers (9–25)
| # | Paper | arxiv | Tags |
|---|-------|-------|------|
| 9 | **CritiqueCrew** | 2602.01796 | agents, seena |
| 10 | **Perspectra** | 2509.20553 | agents, seena |
| 11 | **Hear You in Silence** | 2602.06134 | interviews, seena |
| 12 | **PointAloud** | 2602.09296 | interviews, seena |
| 13 | **Feedback by Design** | 2602.01405 | agents, seena, brainspace |
| 14 | **Scaffolded Vulnerability** | 2602.07508 | brainspace |
| 15 | **Cocoa: Co-Planning and Co-Execution with AI Agents** | 2412.10999 | agents, seena, brainspace |
| 16 | **Scaffolding Metacognition with GenAI** | 2602.09381 | brainspace |
| 17 | **Prototyping Digital Social Spaces** | 2510.02759 | brainspace |
| 18 | **RELATE-Sim** | 2510.00414 | agents, seena |
| 19 | **The AI Memory Gap** | 2509.11851 | eid |
| 20 | **AI Personalization Paradox** | 2601.17846 | eid |
| 21 | **The AI Genie Phenomenon** | 2601.13348 | eid |
| 22 | **Relational Dissonance in Human-AI Interactions** | 2509.15836 | eid |
| 23 | **Proactive Generative AI Agent Roles** | 2602.17864 | agents, eid |
| 24 | **AI and My Values** | 2601.22440 | valuesensitive |
| 25 | **Who Does What? Archetypes of Roles Assigned to LLMs** | 2602.11924 | agents, seena, eid |

### Tag Legend
| Tag | Meaning |
|-----|---------|
| **seena** | Core Seena Labs platform relevance |
| **brainspace** | Brain Space product relevance |
| **eid** | "Everything is Designed" content angle |
| **interviews** | Interview methodology / micro-interviews |
| **behavior** | Behavioral signals and pattern detection |
| **agents** | AI agents and orchestration |
| **qual** | Qualitative research methods |
| **bias** | LLM bias and sycophancy |
| **valuesensitive** | Value-sensitive design |

## How to Continue

### Step 1 — Upload the artifact
Upload `CHI26/index.html` (download from the GitHub repo or from the previous chat) so Claude has the current file to edit.

### Step 2 — Prompt
```
I'm continuing work on my CHI'26 reading list artifact. I've uploaded the current HTML file.

Please read these two skills before starting:
- /mnt/skills/user/hci-paper-reader/SKILL.md
- /mnt/skills/user/seena-design/SKILL.md

Then analyze paper #9 and add it to the artifact following the same pattern as papers 1-8.
```

### Step 3 — Repeat for papers 9–25
Each analysis takes ~1 tool call to search the paper + 1-2 edits to the HTML. You can do 2-3 papers per thread comfortably.

## Architecture of the Artifact

### HTML Structure
- Single-file HTML with embedded CSS and JS
- Password-gated entry via root `index.html` → redirects to `CHI26/index.html`
- All paper data in a `papers` array (id, title, relevance, arxiv, tags, etc.)
- Analyses stored as template literal strings: `paper1Analysis` through `paper8Analysis`
- Analysis routing via `analysisMap` object
- Papers with analyses have `hasAnalysis: true` in the papers array

### To add a new analysis:
1. Create `const paper{N}Analysis = \`...\`;` with the same HTML structure as existing analyses
2. Add it to `analysisMap`: `{ 1: paper1Analysis, ..., N: paper{N}Analysis }`
3. Set `hasAnalysis: true` on the corresponding paper in the `papers` array

### TTS System
- Paragraph-level click-to-speak (`onclick="ttsSpeak(this)"`)
- Play All / Pause / Stop controls per analysis panel
- Uses browser SpeechSynthesis API
- Sticky TTS controls at top of each analysis

### Notes System
- Per-paper notes with types: Note, Idea, Question, Connection, Action
- Notes stored in localStorage and exportable to markdown
- `CHI26/notes/` directory syncs paper note summaries
- Voice dictation support via Web Speech API

### Design System (Seena)
- Background: `#FAFAF8` (cream)
- Accent: `#FF5021` (Seena Orange) — 10% usage on CTAs/highlights
- Secondary: `#2C5F6F` (Deep Teal)
- Typography: IBM Plex Mono for data/tags, DM Sans for UI, Lora serif for analysis body text
- Dark mode support via `data-theme` attribute

### Analysis Section Template
Each analysis follows this exact structure:
1. **Paper Vitals** — Title, authors, venue, one-liner
2. **TL;DR — Why You Should Care** — 2-3 paragraphs, hook + summary
3. **The Core Contribution** — Wobbrock contribution types, methodology detail, strength rating
4. **Paper Evaluation** — Strengths and weaknesses, balanced critique
5. **Similar Reading** — Key references and intellectual lineage
6. **Seena Labs Relevance** — Specific implications for Seena's platform (most important section)
7. **Empirical Evidence Worth Citing** — Stats and numbers for pitches/writing
8. **Everything is Designed — Social Media Angle** — Dinner party version + content hook
9. **Industry vs. Theory** — Practical applicability assessment

### Key CSS Classes
- `.tts-paragraph` — clickable paragraph for TTS
- `.highlight` — key insight callout (teal background)
- `.stat-highlight` — statistical finding callout (orange background)
- `.tts-section-title` — section header within analysis
- `.divider` — section separator

## Linear Tracking
- **EXE-90:** Read and analyze flagged CHI'26 papers
- **EXE-91:** Identify 3 citable stats per paper for pitch deck
- **EXE-92:** Extract Seena-applicable design patterns

## Key Design Decisions Made
- Conversational tone optimized for ~13 min TTS listen per paper
- No bullet points in analysis body (prose only, per skill instructions)
- Seena relevance section is the longest and most detailed
- Each paper gets a Seena relevance rating: CRITICAL / BLUEPRINT / FOUNDATIONAL / HIGH / MODERATE
- Stats use `.stat-highlight` class for visual scanning
