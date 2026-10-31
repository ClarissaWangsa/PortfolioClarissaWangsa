# Project Sheet Structure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Update both portfolio pages so their project data and modal sections match the latest workbook columns.

**Architecture:** Keep the current static HTML architecture and duplicated page-local project data. Replace the old `brief`, `approach`, and `results` fields with workbook-aligned content fields, using shared rendering patterns copied into both pages. Add small pure helpers for parsing workbook text and YouTube URL normalization.

**Tech Stack:** Static HTML, inline CSS, vanilla JavaScript, GitHub Pages, browser verification.

## Global Constraints

- Preserve optional video support: blank `Video Link` means no iframe.
- Preserve the equal-height gallery/video desktop layout and stacked mobile layout.
- Render workbook bullets as semantic `<ul><li>` markup.
- Omit empty content sections and source links.
- Do not add dependencies or convert the static site to a CMS.

---

### Task 1: Align project data with the workbook

**Files:**
- Modify: `index.html:969-1050`
- Modify: `projectpage.html:721-825`

**Interfaces:**
- Produce project objects with `title`, `client`, `year`, `tags`, `overview`, `role`, `challenge`, `execution`, `deliverables`, `outcome`, `source`, `images`, and optional `videoSrc`/`videoType`.

- [ ] **Step 1: Replace old content fields**

For each populated workbook row, replace `brief`, `approach`, and `results` with:

```js
overview: '...',
role: '...',
challenge: '...',
execution: '...',
deliverables: '...',
outcome: '...',
source: '...'
```

Use the three populated rows from the workbook exactly:
- Daihatsu GIIAS, year `2025`, tags `Gamification`, `Interactive`, `Embedded`, no video.
- House of Goodness, year `2025`, tags `Gamification`, `Interactive`, `Embedded`, Shorts video URL.
- Petualangan Paddle Pop, year `2023`, tags `Gamification`, `Roblox`, no video.

- [ ] **Step 2: Correct workbook asset references**

Keep the existing repository asset paths that are present on disk, while retaining the workbook folder names as the source of each project’s gallery path. Ensure each thumbnail resolves to an existing file.

- [ ] **Step 3: Verify data shape in the browser console**

Run:

```js
Object.values(projects).map(p => ({
  title: p.title,
  hasOverview: Boolean(p.overview),
  hasChallenge: Boolean(p.challenge),
  hasExecution: Boolean(p.execution),
  hasDeliverables: Boolean(p.deliverables),
  hasOutcome: Boolean(p.outcome)
}))
```

Expected: all three populated projects have all six content fields.

### Task 2: Add workbook content rendering helpers

**Files:**
- Modify: `index.html` near `buildVideo`
- Modify: `projectpage.html` near `buildVideo`

**Interfaces:**
- `escapeHTML(value)` returns escaped text.
- `formatProjectText(value)` returns text or semantic list HTML, preserving paragraph breaks.
- Existing `getYouTubeEmbedUrl(source)` remains the source for embed normalization.

- [ ] **Step 1: Add safe text formatting**

Implement a helper that escapes `&`, `<`, `>`, `"`, and `'`, converts lines beginning with `-`, `•`, or `*` into `<li>` elements grouped in `<ul>`, and converts remaining paragraphs to `<p>` elements.

- [ ] **Step 2: Add conditional section rendering**

Implement:

```js
function buildContentSection(label, value, extraClass)
```

It returns an empty string for blank values; otherwise it returns a heading and formatted body using the existing `modal-h3` and `modal-body` classes.

- [ ] **Step 3: Add source link rendering**

Implement:

```js
function buildSourceLink(source)
```

It returns an empty string for blank values and otherwise returns a safe external link with `target="_blank"` and `rel="noopener noreferrer"`.

### Task 3: Replace modal markup on both pages

**Files:**
- Modify: `index.html:1054-1095`
- Modify: `projectpage.html:904-938`

**Interfaces:**
- `openModal(id)` renders the same section order on both pages.

- [ ] **Step 1: Keep the media row**

Retain the current `.project-media-grid` markup and optional `buildVideo(p)` output.

- [ ] **Step 2: Update metadata**

Render Year, My Role, Client, and Tags below the media row. Remove the obsolete Project Type metadata because the workbook has `Tag` instead.

- [ ] **Step 3: Render workbook sections**

Render in this order:

```js
buildContentSection('Project Overview', p.overview),
'<div class="project-copy-grid">' +
  buildContentSection('The Challenge', p.challenge) +
  buildContentSection('Project Execution', p.execution) +
'</div>',
buildContentSection('Project Deliverables', p.deliverables),
buildContentSection('Project Outcome', p.outcome),
buildSourceLink(p.source)
```

- [ ] **Step 4: Remove old results rendering**

Delete `resultsHTML`, `result-card`, and `modal-result-list` usage from the modal functions. Leave unrelated homepage styling untouched unless the removed markup makes a rule unused.

### Task 4: Verify the updated project experience

**Files:**
- Verify: `index.html`
- Verify: `projectpage.html`

- [ ] **Step 1: Run the local static server**

Run:

```bash
python3 -m http.server 8000
```

- [ ] **Step 2: Verify the homepage modal**

Open `http://127.0.0.1:8000/index.html`, open all three projects, and verify:
- each title, year, role, client, and tags match the workbook;
- all six new headings appear;
- Daihatsu and Paddle Pop have no video iframe;
- Fitbar has an iframe whose source contains `/embed/WBEU8KRVFvg`;
- Fitbar’s gallery and video rectangles have equal height.

- [ ] **Step 3: Verify the all-projects modal**

Repeat the same checks at `http://127.0.0.1:8000/projectpage.html`, including filter visibility and source links.

- [ ] **Step 4: Run static checks**

Run:

```bash
git diff --check
```

Then run IDE lint diagnostics for both HTML files and fix any newly introduced errors.
