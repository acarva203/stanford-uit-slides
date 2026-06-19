---
name: stanford-uit-slides
description: Creates Stanford University IT branded PowerPoint (.pptx) presentations. Use whenever the user asks for a "slide deck", "slides", "presentation", "deck", or "Stanford UIT deck". MUST ask the user which skill to use (stanford-uit-slides vs generic pptx) before building. Produces brand-compliant .pptx files from four official Stanford UIT templates (Sunset, Noe, Castro, Mission), applying Stanford identity guidelines automatically. Use even when the user only says "make me slides about X" if they're in a Stanford UIT context. Invite the user to look at the templates available at https://uitcommunity.stanford.edu/university-it-identity#section_1361 to help decide.

---

# Stanford UIT Slides Skill

Produces `.pptx` files matching the four official Stanford UIT PowerPoint templates.
**This file is the WHAT. The HOW lives in [`skill-complementary.md`](skill-complementary.md)** — exact build steps, EMU math, python-pptx recipes, asset paths, and the compliance evaluator.

## 0. Before you start

**Ask the user:** "Would you like me to use the Stanford UIT template skill, or the generic pptx skill?" Then proceed based on their answer.

---

## 1. Templates (grounded in the actual .pptx files)

All four source files live in `reference-materials/original-templates/`. Build by using those files as the `pptx` skill template base — do NOT reconstruct from scratch.

| ID | File | Aspect | Size (in) | Slide layouts | Best for |
|---|---|---|---|---|---|
| **sunset** | `Powerpoint Slide Template A-Sunset.pptx` | 16:9 | 10×5.625 | 25 layouts | General purpose; **default** |
| **noe** | `Powerpoint Slide Template B-Noe.pptx` | 16:9 | 10×5.625 | 11 layouts | Technical / data-dense |
| **castro** | `Powerpoint Slide Template C-Castro.pptx` | 16:9 | 10×5.625 | 23 layouts | Executive / high-impact |
| **mission** | `Powerpoint Slide Template D-Mission.pptx` | 16:10 | 10×6.25 | 21 layouts | Narrative / storytelling |

EMU dimensions: 16:9 = `9,144,000 × 5,143,500`; Mission = `9,144,000 × 5,715,000`.

### 1.1 Template auto-selection

| Keywords in brief | Template |
|---|---|
| executive, leadership, pitch, board, high-impact | castro |
| data, metrics, analysis, technical, detailed | noe |
| mission, vision, culture, storytelling, strategic narrative | mission |
| update, briefing, routine, general, service | sunset |
| *uncertain / not matched* | **sunset** |

Explicit user override wins over all keyword rules.

---

## 2. Available layouts per template (real names from the .pptx files)

### Sunset & Castro (shared layout vocabulary, different styling)

| Layout name | Use for |
|---|---|
| `Title Slide txt only` | Cover — text only |
| `Title Slide w/image` | Cover — with full-bleed or half image |
| `Title Slide w/txt & graphic elements` | Cover — with decorative Stanford graphics |
| `Title Slide w/txt on white` | Cover — clean white background |
| `Title and Content` | Standard content slide |
| `Title and Content w/image` | Content + right-side image |
| `TWO_OBJECTS` | Two-column content |
| `Objective Slide` | Three-row objective / goals layout |
| `Data slide` | Data + metrics layout |
| `Three story inset w/images` | 3-column image + caption |
| `Three story inset w/images (Cardinal bkgd)` | Same on Cardinal background |
| `Divider - Sky` / `Poppy` / `Palo Verde` / `Plum` / `Illuminating (Dark)` | Color-block section dividers |
| `Divider - Thank You` | Closing slide |
| `TITLE_ONLY` | Title bar only, free content area |
| `BLANK` | Fully blank slide |

Castro also has `SLIDE 6` (agenda-style layout).

### Noe layouts

| Layout name | Use for |
|---|---|
| `TITLE` | Cover / title slide |
| `SECTION_HEADER` | Section divider |
| `TITLE_AND_BODY` | Standard content |
| `TITLE_AND_TWO_COLUMNS` | Two-column layout |
| `ONE_COLUMN_TEXT` | Single wide text column |
| `MAIN_POINT` | Big-statement slide |
| `SECTION_TITLE_AND_DESCRIPTION` | Section intro |
| `CAPTION_ONLY` | Image/chart with caption |
| `BIG_NUMBER` | Stat callout (large number + label) |
| `TITLE_ONLY` / `BLANK` | Utility layouts |

### Mission layouts

| Layout name | Use for |
|---|---|
| `Title Slide txt only` / `w/image` / `w/image + graphic elements` / `w/txt & graphic elements` / `w/txt on white` | Cover variants |
| `Title and Content` | Standard content |
| `Title and Content w/bkgd` | Content on tinted background |
| `Title and Content w/image` | Content + image column |
| `Two Column Content` | Side-by-side columns |
| `Data slide` | Data/metrics |
| `Objective Slide` | Three-row objectives |
| `Three story inset w/images` / `(Cardinal bkgd)` | 3-column image+caption |
| `Divider - Poppy/Illuminating (Dark)/Palo Verde/Sky/Plum` | Section dividers |
| `Divider - Thank You` | Closing slide |
| `Title Only` / `Blank` | Utility |

---

## 3. Brand standards (from actual template files)

### 3.1 Typography (verified from source .pptx)

| Template | Headlines | Body copy | Fallbacks |
|---|---|---|---|
| Sunset | Georgia | Source Sans Pro | Calibri, Roboto |
| Noe | Georgia | Source Sans Pro | Calibri, Roboto, Roboto Condensed |
| Castro | Georgia | Source Sans Pro | Calibri, Roboto, Comfortaa |
| Mission | Georgia | Calibri | (system fonts only) |

For generated slides, use **Source Sans 3** (primary body) with **Roboto** as fallback for Sunset/Noe/Castro; **Calibri** for Mission body.

### 3.2 Color palettes (verified from source .pptx)

**Shared Stanford brand colors (all templates):**
- Cardinal Red: `#8C1515`
- Lagunita (teal): `#007C92`
- Black: `#2E2D29`
- White: `#FFFFFF`
- Cool Grey: `#4D4F53`
- Warm Grey: `#7F7776`
- Light Grey: `#E7E6E6`

**Extended per-template palettes:**

| Color name | Hex | Templates |
|---|---|---|
| Poppy (orange) | `#E98300` | All |
| Illuminating (gold) | `#FEC51D` | All |
| Palo Verde (green) | `#279989` | All |
| Plum | `#620059` | All |
| Sky (light blue) | `#67AFD2` | Sunset, Castro |
| Robin (lighter teal) | `#8FDAFF` | Castro |

### 3.3 Logos
Six official University IT wordmarks in `shared-assets/logos/`:
- `University-IT_wordmark_horizontal_black.png`
- `University-IT_wordmark_horizontal_cardinal-black.png`
- `University-IT_wordmark_horizontal_white.png`
- `University-IT_wordmark_vertical_black.png`
- `University-IT_wordmark_vertical_cardinal-black.png`
- `University-IT_wordmark_vertical_white.png`

Minimum size: 1 inch (per brand-compliance.json). Place in footer area with clear space.

---

## 4. Presentation types and slide sequences

| Type | Slide sequence |
|---|---|
| Technical Briefing | Cover → Agenda → Current State → Proposed Changes → Timeline → Next Steps → Thank You |
| Security Training | Cover → Objectives → Policies → Best Practices → Demo/Examples → Assessment → Resources → Thank You |
| Service Update | Cover → Summary → Impact → Timeline → User Actions → Support → Thank You |
| Project Proposal | Cover → Problem → Solution → Implementation → Budget → Timeline → Thank You |
| Incident Report | Cover → Summary → Impact Timeline → Root Cause → Prevention → Thank You |

---

## 5. Build workflow

```
1. Clarify template (auto-select or user override)
2. Read skill-complementary.md for exact build steps
3. Load source .pptx from reference-materials/original-templates/
4. Select layouts from template's real layout list (§2)
5. Populate placeholders (titles, body, images)
6. Apply brand colors and fonts (§3)
7. Add logo to footer
8. Run compliance check (see skill-complementary.md §8)
9. Output final .pptx
```

**Always use the original .pptx as template base** — this preserves the slide master, theme colors, and embedded graphic elements.

---

## 6. Fallbacks

- Uncertain template → **sunset**
- Missing Source Sans 3 → Roboto → Calibri
- Missing asset → log and degrade gracefully (see skill-complementary.md §7)
- Compliance score < 85 → flag for review; < 70 (critical) → block generation