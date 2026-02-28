# CHI'26 Reading List — Continuation Brief

## What This Is
An interactive HTML reading list of CHI 2026 HCI+AI preprints relevant to Seena Labs, with embedded paper analyses optimized for TTS (text-to-speech) listening. The artifact is styled with Seena's design system and lives at:

- **GitHub repo:** https://github.com/thaxali/HCI.git  
- **GitHub Pages:** https://thaxali.github.io/HCI (once Pages is enabled, file is `index.html`)
- **Artifact filename:** `chi26-seena-reading-list.html`

## Current Status — 3 of 8 papers analyzed

### ✅ Completed Analyses
| # | Paper | arxiv | Seena Relevance |
|---|-------|-------|-----------------|
| 1 | **Sensing What Surveys Miss** — Ailin Liu et al. | 2502.04455 | CRITICAL — Detection agent trigger logic, threshold adaptation (6 traversal paths), mouse movement features, 21% accuracy boost, timing = 40% of acceptance variance |
| 2 | **InterFlow** — Yi Wen et al. | 2502.07459 | BLUEPRINT — Closest CHI analog to Seena's micro-interview system. Actionability gap = Seena's structural advantage (AI agent IS the interviewer), graduated automation model, speaking ratio metrics |
| 3 | **Behavioral Indicators of Overreliance** — Chang Liu et al. | 2602.11567 | FOUNDATIONAL — Autoencoder+DBSCAN pipeline for behavioral clustering, 5 behavioral patterns (hesitation-then-acceptance is gold for micro-interview timing), fine vs coarse navigation as engagement proxy |

### ⬜ Remaining Papers to Analyze
| # | Paper | arxiv | Tags |
|---|-------|-------|------|
| 4 | **Crowdsourcing With AI** — Yen-Chia Hsu et al. | 2502.08868 | crowdsourcing, ai |
| 5 | **When the Rubber Meets the Road** — Nikolas Martelaro et al. | 2502.07129 | agents, llm |
| 6 | **Steering Without Sight** — Nikolas Martelaro et al. | 2502.06836 | agents, llm |
| 7 | **AI Agents That Matter** — Sayash Kapoor et al. | 2407.01502 | agents, evaluation |
| 8 | **Evaluating Agent-Based Program Repair** — Thien Nguyen et al. | 2405.02183 | agents, evaluation |

## How to Continue

### Step 1 — Upload the artifact
Upload `chi26-seena-reading-list.html` (download from the GitHub repo or from the previous chat) so Claude has the current file to edit.

### Step 2 — Prompt
```
I'm continuing work on my CHI'26 reading list artifact. I've uploaded the current HTML file. 

Please read these two skills before starting:
- /mnt/skills/user/hci-paper-reader/SKILL.md
- /mnt/skills/user/seena-design/SKILL.md

Then analyze paper #4 and add it to the artifact following the same pattern as papers 1-3.
```

### Step 3 — Repeat for papers 5-8
Each analysis takes ~1 tool call to search the paper + 1-2 edits to the HTML. You can do 2-3 papers per thread comfortably.

## Architecture of the Artifact

### HTML Structure
- Single-file HTML with embedded CSS and JS
- All paper data in a `papers` array (id, title, authors, arxiv, tags, seenaRelevance, etc.)
- Analyses stored as template literal strings: `paper1Analysis`, `paper2Analysis`, `paper3Analysis`
- Analysis routing via `analysisMap` object: `{ 1: paper1Analysis, 2: paper2Analysis, 3: paper3Analysis }`
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

### Design System (Seena)
- Background: `#F6F5F4` (cream)
- Accent: `#FF5021` (Seena Orange) — 10% usage on CTAs/highlights
- Secondary: `#2C5F6F` (Deep Teal)
- Tertiary: `#7C9885` (Sage Green) — read states
- Typography: IBM Plex Mono for data/tags, Lora serif for analysis body text
- Cards: `rounded-3xl`, shadow hierarchy, 200ms animations

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
