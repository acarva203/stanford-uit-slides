# stanford-uit-slides — Build Procedure (Complement to SKILL.md)

> **This file supplies the HOW.** `SKILL.md` describes *what* the
> `stanford-uit-slides` skill is, *which* templates exist, *which* content
> structures it offers, and the *brand promises* it makes. It contains **no
> build procedure**. This document is the executable companion: exact input
> paths, the end-to-end generation procedure with selector citations, the CSS→EMU
> unit-translation rules, per-slide-type python-pptx recipes, branding
> application, the asset-integrity status (now mostly remediated — see §7), and a
> runnable compliance
> evaluator. All values are quoted from repo files; every in-repo conflict is
> flagged explicitly (see §8.4) rather than silently resolved.

---

## §1. Purpose & relationship to SKILL.md

- **SKILL.md** = declarative spec (templates, content types, brand promises).
- **This file** = imperative build steps an agent can follow without re-deriving
  any value.
- Read order: SKILL.md first (context), then this file (execution).
- This file does not duplicate SKILL.md or invent layouts. Every layout key
  named here exists in the corresponding `template-configs/<tpl>/layouts.json`.

---

## §2. Inputs (paths relative to the skill root)

All paths below are relative to the skill's root directory (the folder
containing `SKILL.md`). Every referenced asset resolves on disk (see §7).

| Purpose | Path |
|---|---|
| Skill spec | `SKILL.md` |
| Template selector logic | `integration/template-selector.json` |
| Brand colors / typography + asset references (master) | `shared-assets/stanford-uit-branding.json` |
| Compliance rules / scoring | `validation/brand-compliance.json` |
| Font filename mappings | `shared-assets/font-mappings.json` |
| PPTX handoff / canvas config | `integration/pptx-handoff-config.json` |
| Per-template theme | `template-configs/<tpl>/theme.json` |
| Per-template layouts | `template-configs/<tpl>/layouts.json` |
| Per-template master slides | `template-configs/<tpl>/master-slides.json` |
| Per-template metadata | `template-configs/<tpl>/metadata.json` |
| Fonts on disk | `shared-assets/fonts/` |
| Logos on disk | `shared-assets/logos/` |
| Icons on disk | `shared-assets/icons/` |

where `<tpl>` ∈ {`sunset`, `noe`, `castro`, `mission`}.

> **Path note (changed):** the recent "incorrect file references" commits moved
> several files. `template-selector.json` now lives under **`integration/`**
> (not `validation/`). The former `validation/stanford-uit-branding.json` and the
> separate `shared-assets/config/branding.json` are now a **single merged file**,
> `shared-assets/stanford-uit-branding.json`, which holds brand colors,
> typography, logo references, and service-icon references together. There is no
> longer a `shared-assets/config/` directory; `font-mappings.json` is now at
> `shared-assets/font-mappings.json`.

> **Canvas note (corrected — verified against the source `.pptx`):** all three
> 16:9 templates are **10in × 5.625in** (`9,144,000 × 5,143,500 EMU`) — the legacy
> PowerPoint 16:9 size, read directly from each file's
> `p:sldSz cx="9144000" cy="5143500"`. Earlier drafts of this file and the configs
> claimed `13.33in × 7.5in`; that was **wrong** and has been corrected here, in the
> three `theme.json`, and in `integration/pptx-handoff-config.json`. Mission is
> 16:10 at **10in × 6.25in** (`9,144,000 × 5,715,000 EMU`). See §4 for the
> unit math on the true canvas.

---

## §3. Template selection procedure (cited to `integration/template-selector.json`)

Apply in precedence order; first match wins, then validate against audience
avoid-lists.

**Step 1.1 — explicit template request.** If the caller names a template, use it
(subject to Step 1.5 audience avoidance warning).

**Step 1.2 — presentation_type_mapping.** Look up the requested type in
`presentation_type_mapping`. Verified primaries:
- `incident_report.primary = noe`
- `strategic_planning.primary = mission`
- `service_update.primary = sunset`
- `technical_briefing.primary = sunset` *(NOT noe — common mistake)*
- `security_training.primary = noe`

**Step 1.3 — keyword_triggers** (fall back to keyword scan of the brief):
- `executive, leadership, pitch, strategy, high-level, board` → **castro**
- `standard, general, briefing, update, routine, regular` → **sunset**
- `data, technical, metrics, analysis, detailed, research` → **noe**
- `mission, vision, …, culture` → **mission**

**Step 1.4 — audience_preferences.** e.g. `executive_leadership`:
preferred `[castro, mission]`, avoid `[noe]`. If a Step 1.2/1.3 result lands on
an avoided template for the stated audience, warn and prefer an allowed one.

**Step 1.5 — fallback.** `default` / `uncertain` → **sunset**.

> Regression coverage exists: tests `selection_001`, `selection_002`, `font_001`
> in the validation suite assert the above values.

---

## §4. Unit translation (CSS px / % / in → python-pptx EMU)

python-pptx geometry uses **EMU**. Constants:
`1 inch = 914400 EMU`, `1 pt = 12700 EMU`.

### §4.1 Units the layouts actually use
The 16:9 `layouts.json` files are **percentage / keyword based** (`"100%"`,
`"80%"`, `"center"`, `"38%"`), with only small absolute `px` values for
decorative offsets (borders, gaps — none exceed ~110px). There is **no
1280×720 grid**; that claim in earlier drafts was fabricated. Mission declares an
explicit **1000×625 px grid** (`mission/theme.json` `px_width/px_height`), i.e.
**100 px/in**, so for Mission `1 px = 9144 EMU`. Apply the same 100 px/in to the
stray px offsets in the other templates unless a layout says otherwise.

Resolution rules:
- **`%`** → fraction × the **canvas EMU** for that axis (see §4.2).
- **`px`** → `px × 9144 EMU` (100 px/in grid).
- **`center` / keywords** → compute from element size against the canvas.

### §4.2 Canvas EMU (verified from each source `.pptx`)
| Template(s) | Inches | Width EMU | Height EMU |
|---|---|---|---|
| Sunset / Noe / Castro (16:9) | 10 × 5.625 | **9,144,000** | **5,143,500** |
| Mission (16:10) | 10 × 6.25 | **9,144,000** | **5,715,000** |

All four templates are **10in (9,144,000 EMU) wide**; only the height differs.
Do not use 13.33in / 12,192,000 EMU anywhere — that canvas does not exist in
these files.

### §4.3 % → EMU
`pct_of_width × 9,144,000` (all templates, since width is uniform), and
`pct_of_height ×` 5,143,500 (16:9) or 5,715,000 (Mission).

### §4.4 Worked `calc()` examples (recomputed on the true 10in canvas)
At 100 px/in (`1px = 9144 EMU`, `width = 9,144,000 EMU`):

| Field | CSS | Inches | EMU |
|---|---|---|---|
| `two_column` right_column x | `calc(52% + 0px)` | `0.52×10 = 5.2in` | **4,754,880** |
| `content_slide` content_area w | `calc(100% − 80px)` | `10in − 0.8in = 9.2in` | **8,412,480** |

> **Flag:** these replace the old 13.333in-basis figures (6,339,682 /
> 11,429,714), which were computed against a canvas width that does not exist.
> The px term uses the 100 px/in grid Mission declares; if a 16:9 layout proves
> to use a different dpi for its px offsets, recompute that term accordingly.

### §4.5 Verified spacing/logo constants (no change)
`76200`, `762000`, `380990 ≈ 381000`, `1047750`, `2606040`; logo dims
`0.625in` / `1.6667in`. Mission `px/100` conversions and all Mission coordinates
match `template-configs/mission/layouts.json`.

---

## §5. Per-slide-type python-pptx recipes

For each template, read `layouts.json` for the named layout, translate each
element's `x/y/w/h` per §4, set fonts/colors per §6, then place text/shapes.

### §5.1 Key per-template values (cite the layout, not the theme)
- **Noe** title slide `overline_label`: **11pt** — source
  `noe/layouts.json` (overline_label `font_size "11pt"`). Note `noe/theme.json`
  `label_font` is 10pt; if challenged, cite the **layout**.
- **Castro** `stat_callout` → `stat_number`: **64pt**, family `Source Sans 3`
  (fallback Roboto), weight 900, color **`#FFFFFF`** — source `castro/layouts.json`.
  The card's `border_left` carries the cardinal accent (`#8C1515`); the number
  itself is white because cardinal-on-dark fails contrast. (Castro's accent was
  swapped from gold to Stanford cardinal — see `castro/theme.json` `extraction_note`.)
  > **Flag:** `castro/theme.json` `stat_font` declares **72pt** (line 57). This
  > 72pt-vs-64pt divergence is a real in-repo conflict (see §8.4); render from
  > the layout (64pt) and flag.

### §5.2 Mission dividers
Mission uses full-bleed section dividers. Use the `layout_index` keys
1/2/6/9/10/14/15/19 from `mission/layouts.json` (the JSON field is literally
`layout_index`). Divider background/text colors and all cited coordinates match
that file. Mission has **21** layouts total (consistent with its metadata).

---

## §6. Branding application

### §6.1 Profiles (from `integration/pptx-handoff-config.json`)
All four profiles' `primary_logo`, `color_emphasis`, and `typography_weight`
match the handoff config exactly; the narrative profile's `special_features`
are as cited. Apply the profile matching the chosen template.

### §6.2 Colors (verified hexes)
All hexes match `shared-assets/stanford-uit-branding.json` and
`mission/theme.json` `brand_colors`. Prohibited-color set matches
`validation/brand-compliance.json`:
`prohibited_colors.low_contrast = ["#CCCCCC", "#EEEEEE"]` (line 30).

---

## §7. Asset-integrity status (remediated — all referenced assets resolve)

Every asset reference now resolves on disk. This section records the resolution
for traceability.

### §7.1 Logos — resolved
All 6 logo `file_path` entries in `shared-assets/stanford-uit-branding.json` point
to real PNGs in `shared-assets/logos/`, including the two cardinal variants
(`*_cardinal-black.png`, hyphenated). No substitution needed.

### §7.2 Fonts — resolved
`shared-assets/font-mappings.json` references `fonts/SourceSans3-{Light,Regular,SemiBold,Bold}.ttf`
and `fonts/Roboto-Regular.ttf` — all present, all relative paths (no leading
slash), all `.ttf` (the web-only `.woff2` set was removed). PowerPoint embeds
`.ttf`/`.otf`, so this is the correct format.

### §7.3 Icons — resolved
The obsolete `status_indicators` icon refs (`check/wrench/alert.svg`) were removed.
The remaining `service_icons` — `email-icon.svg`, `wifi-icon.svg`,
`security/shield-icon.svg` — all exist. (Disk also carries unreferenced icons:
`vpn`, `lock`, `warning`, `cloud`, `server`.)

---

## §8. Compliance evaluator (runnable, cited to `brand-compliance.json`)

### §8.1 Prohibited-color check (background-aware)
Do **not** flag colors by hex alone. `prohibited_colors.low_contrast` is now a
**background-aware rule**: compute the WCAG contrast ratio of each
text/background pair (§8.2) and flag only pairs below **4.5:1** (3:1 large text).
`#CCCCCC` / `#EEEEEE` are listed as `commonly_failing_on_light_backgrounds` — they
are *compliant* on dark backgrounds.

> **Resolved:** Castro's `#CCCCCC` body sits on `#000000` (~15:1) — it **passes**
> the ratio check and is no longer flagged. The earlier "flags every Castro slide"
> problem was the flat hex list, which has been replaced.

### §8.2 WCAG contrast (formula verified)
Relative luminance per channel `c`: `cs = c/255`; `lin = cs/12.92 if cs ≤
0.03928 else ((cs+0.055)/1.055)**2.4`; `L = 0.2126R + 0.7152G + 0.0722B`;
ratio `(Llight+0.05)/(Ldark+0.05)`. Verified ratios:

| Pair | Ratio | Verdict |
|---|---|---|
| Cardinal text | 9.40 | PASS |
| Success | 7.59 | PASS |
| Error | 7.26 | PASS |
| Warning gold | 2.03 | **FAIL** |
| Black on gold | 10.32 | PASS |
| Dark on illuminating | 8.77 | PASS |

### §8.3 Scoring (matches `brand-compliance.json`)
Deductions: **−50** (critical), **−20** (major), **−5** (minor).
`passing_score = 85`, `excellence_score = 95`. Escalation strings:
`block_presentation_generation`, `alert_uit_design_team`, halt-on-critical.

### §8.4 In-repo conflicts to reconcile (DO NOT silently resolve)
1. **Castro logo placement:** `castro/theme.json` allows `bottom_left`, but
   `brand-compliance.json` `prohibited_usage.placement` prohibits it.
2. **Logo minimum size:** `stanford-uit-branding.json`
   (`minimum_width = "2_inches_horizontal_1.5_inches_stacked"`) says 2in/1.5in;
   `brand-compliance.json` says 1in.
3. **Cool Grey — RESOLVED:** `stanford-uit-branding.json` `cool_grey = #4D4F53` is
   now the canonical value (see its `palette_authority` note); Mission's
   `#53565A` occurrences were reconciled to `#4D4F53`. Use the canonical value
   for any shared-semantic color.
4. **Fonts standardized:** all templates now use **Source Sans 3** (primary) with
   **Roboto** (fallback). Earlier per-template fonts (Georgia/Calibri/Arial Black)
   have been removed from the configs; verify `approved_fonts` lists Source Sans 3
   and Roboto.
5. **Castro stat font size:** theme.json 72pt vs layouts.json 64pt (§5.1).
6. **Castro `#CCCCCC` body text — RESOLVED:** the `low_contrast` check is now
   background-aware (§8.1), so `#CCCCCC` on Castro's dark background passes and is
   no longer a conflict.

---

## §9. Edge cases & fallbacks
- Missing asset → apply §7 substitution (cardinal logo → `*_cardinal-black.png`;
  strip leading slash and resolve fonts under `shared-assets/fonts/`). If a
  referenced asset genuinely has no on-disk match, degrade gracefully and log.
- Selection uncertain → sunset (§3 Step 1.5).
- Unit convention → pick px-grid **or** inch-exact (§4.2) and stay consistent.
- Prohibited-color / placement / size conflicts (§8.4) → surface to the caller;
  do not auto-resolve.

## §10. Critical Criteria to Confirm
- Is there a logo on each slide with at least a divider or whitespace separating from the rest of the slide? Is the logo correctly sized? Make sure the logo is not vertically or horizontally compressed. If the logo is present, there is no need to mention "Stanford University IT" on the slide.
- Are all icons correctly sized within their elements, and properly aligned?
- Make sure the slide deck uses Source Sans 3 as the primary font and Roboto as the supplementary font.
- Are the primary colors for the slide deck cardinal, black, white, or soft grey?