# Project Sheet Structure Design

## Goal

Make the portfolio project cards and modal content follow the latest `Project - Portfolio 2026 Clarissa.xlsx` structure while preserving optional video and gallery support.

## Data model

Each project uses the workbook columns:

- `Order`, `Homepage`, `Nama Project`, `Client`, `Year`, `Tag`
- `Project Overview`, `My Role`, `The Challenge`, `Project Execution`
- `Project Deliverables`, `Project Outcome`
- `Source`, `Thumbnail foto`, `Video Link`, `Foto folder`

`Homepage = Yes` controls whether a project appears on the homepage. Empty `Video Link` values produce a photo-only layout. YouTube Shorts, watch, short-link, and embed URLs are normalized to a standard embed URL before rendering.

## Modal layout

1. Project title and client.
2. Gallery plus optional 9:16 video, with equal media height.
3. Full-width metadata row containing Year, My Role, Client, and Tags.
4. `Project Overview` as a full-width section.
5. `The Challenge` and `Project Execution` as two columns on desktop, stacked on mobile.
6. `Project Deliverables` as a full-width bullet list.
7. `Project Outcome` as a full-width section.
8. Source link at the bottom when the workbook provides one.

The existing result-card treatment is removed because the workbook now stores outcomes as prose/list content rather than separate numeric result objects.

## Rendering rules

- Workbook line breaks and bullet prefixes become semantic HTML list items.
- Empty fields are omitted rather than rendered as blank headings.
- Thumbnail and gallery paths continue to resolve relative to each project media folder.
- The same rendering/data shape is used in `index.html` and `projectpage.html` to keep both modal experiences consistent.

## Verification

- Check all three populated workbook projects render.
- Verify Daihatsu and Paddle Pop have no video column and use the photo-only layout.
- Verify Fitbar converts its Shorts URL to an embed URL and renders the iframe.
- Verify the new section headings and source link are present.
- Run browser checks, `git diff --check`, and IDE lint diagnostics.
