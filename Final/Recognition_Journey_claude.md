# My Recognition Journey — Prototype Documentation

## Overview
Recognition Journey is an annual, story-driven experience built for Vantage Circle that transforms employee recognition data into a beautiful, navigable digital book.

---

## Prototype

**File:** `index.html`
**Run:** Serve the `Final/` folder with any local HTTP server.

```bash
cd "Final"
python3 -m http.server 8080
# Open: http://localhost:8080/index.html
```

---

## Landing Page

**Source image:** `Discovery Point.png`

Displays the Vantage Circle feed with a notification card:
> *Your 2025 Recognition Journey is Ready!*

A transparent clickable overlay (CSS percentage-based, no JS positioning) covers the full notification card area. Clicking anywhere on it — including the "Open Journey" button — launches the full-screen story viewer.

---

## Story Viewer

Full-screen dark overlay (`rgba(0,0,0,0.93)`) with 9 slides shown in sequence.

| # | File | Content |
|---|------|---------|
| 1 | `1 Cover.png` | Book cover — "Recognition Journey", 2025 Edition *(updated 30 Jun 2026 — removed name, joining date, and "Stories of Impact" subtitle)* |
| 2 | `2 Your Impact.png` | 28 recognitions, 15 people, 1,248 words, 4 values |
| 3 | `3 Your Recognition Milestones.png` | Year-by-year timeline 2020 → 2025 |
| 4 | `Recognitions from People.png` | Manager (12), Peers (10), Cross Team (5), Direct Reports (1) |
| 5 | `4 Values You Represent.png` | Innovation, Teamwork, Ownership, Customer First |
| 6 | `5 Badge Cabinet.png` | 16 badges: Team Player, Culture Builder, Problem Solver, AI Trailblazer, Innovator, Customer Hero |
| 7 | `6 Award Cabinet.png` | 11 awards: Spot, Excellence, Above & Beyond, Leadership, Innovation, Customer Impact |
| 8 | `7 How You Built Culture.png` | 84 given, 47 recognised, 126 comments, 389 likes |
| 9 | `8 End Page.png` | Closing — "Your Journey Inspires Many" |

---

## Navigation

| Action | Result |
|--------|--------|
| Click notification card / Open Journey | Opens story viewer |
| → Arrow key or right arrow button | Next slide |
| ← Arrow key or left arrow button | Previous slide |
| Click right half of image | Next slide |
| Click left half of image | Previous slide |
| Click progress segment | Jump to that slide |
| ESC or ✕ button | Close viewer |

---

## Download for Social Sharing

On the **last slide (End Page)**, a purple **"Save for Social Sharing"** button appears below the slide.

### Flow
1. User reaches slide 9 → download CTA becomes visible
2. Click **"Save for Social Sharing"** → opens the Download Panel
3. Download Panel shows all 9 slides as a scrollable 3-column card grid
4. Each card shows a full thumbnail (no cropping) with a **"↓ Save"** button
5. **"Download All"** downloads all 9 slides in sequence with a short delay between each
6. **"Back to Journey"** or **ESC** returns to the story viewer

### Implementation details
- Downloads use `fetch()` → `Blob` → `URL.createObjectURL()` → programmatic `<a>` click
- Filenames are formatted as `Recognition_Journey_<SlideName>.png`
- Thumbnails use `object-fit: contain` (not `cover`) so no image content is cropped
- Panel uses `display: block; overflow-y: auto` to ensure full scrollability at all viewport sizes
- A centred `#panel-inner` wrapper (max-width 760px) handles the layout inside the scrollable panel

---

## UI Specs

- Story aspect ratio: 9:16
- Font: Inter
- Primary Purple: `#5B3DF5`
- Accent Gold: `#F5C84C`
- Corner radius: 16–20px
- Story viewer background: `rgba(0,0,0,0.93)`
- Download panel background: `rgba(0,0,0,0.96)`
- Thumbnail background (letterbox): `#111`

---

## Known Fixes Applied

| Issue | Fix |
|-------|-----|
| "Open Journey" overlay not clickable | Changed from JS-calculated `position: fixed` to CSS percentage `position: absolute` within image container |
| Story viewer not opening | `overlay` variable was undefined after JS refactor — added `const overlay = document.getElementById(...)` |
| Download panel cards cropped | `object-fit: cover` replaced with `object-fit: contain` |
| Download panel cut off at bottom | Replaced `display: flex; overflow-y: auto` with `display: block; overflow-y: auto` + inner wrapper |
| Cover image updated | `1 Cover.png` replaced on 30 Jun 2026 — title simplified to "Recognition Journey", removed employee name, joining date, and "Stories of Impact" line. Filename unchanged; hard refresh (`Cmd+Shift+R`) picks up the new version. |

---

## Files in This Folder

```
Final/
├── index.html                                     ← Prototype entry point
├── Discovery Point.png                            ← Landing page image
├── Recognition Journey.png                        ← All-slides overview
├── 1 Cover.png
├── 2 Your Impact.png
├── 3 Your Recognition Milestones.png
├── Recognitions from People.png
├── 4 Values You Represent.png
├── 5 Badge Cabinet.png
├── 6 Award Cabinet.png
├── 7 How You Built Culture.png
├── 8 End Page.png
├── Recognition_Journey_Feature_Specification.md   ← Original product spec
└── Recognition_Journey_claude.md                  ← This file
```
