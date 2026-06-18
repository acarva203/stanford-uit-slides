# stanford-uit-slides — Build Procedure (Complement to SKILL.md)

> **This file supplies the HOW.** `SKILL.md` describes *what* the
> `stanford-uit-slides` skill is, *which* templates exist, *which* content
> structures it offers, and the *brand promises* it makes. It contains **no
> build procedure**. This document is the executable companion: exact input
> paths, the end-to-end generation procedure with selector citations, the CSS→EMU
> unit-translation rules, per-slide-type python-pptx recipes, branding
> application, the asset-integrity remediation tables, and a runnable compliance
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

## §2. Inputs (exact paths — all verified to exist)

Repo root: `/Users/asirur/claude_skills/project1_uit/stanford-uit-slides`

| Purpose | Path |
|---|---|
| Skill spec | `SKILL.md` |
| Template selector logic | `validation/template-selector.json` |
| Brand colors / typography (master) | `validation/stanford-uit-branding.json` |
| Compliance rules / scoring | `validation/brand-compliance.json` |
| Font filename mappings | `shared-assets/config/font-mappings.json` |
| Branding asset references | `shared-assets/config/branding.json` |
| PPTX handoff / canvas config | `integration/pptx-handoff-config.json` |
| Per-template theme | `template-configs/<tpl>/theme.json` |
| Per-template layouts | `template-configs/<tpl>/layouts.json` |
| Per-template master slides | `template-configs/<tpl>/master-slides.json` |
| Per-template metadata | `template-configs/<tpl>/metadata.json` |
| Fonts on disk | `shared-assets/fonts/` |
| Logos on disk | `shared-assets/logos/` |
| Icons on disk | `shared-assets/icons/` |

where `<tpl>` ∈ {`sunset`, `noe`, `castro`, `mission`}.

> **Canvas note (16:9):** `integration/pptx-handoff-config.json` declares
> `"width": "13.33in"` (lines 11/20/29 for the 16:9 templates; line 38 is the
> 10in 4:3 variant). See §4.2 for the **13.33-vs-13.333-vs-px-grid** nuance —
> do not treat 12,192,000 EMU as the inch-exact width.

---

## §3. Template selection procedure (cited to `template-selector.json`)

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

### §4.1 px → EMU
The configs are authored on a **1280×720 px grid mapped to a 13.333in canvas**,
so the px→EMU factor is `13.333/1280 in` per px. Two conventions coexist in the
repo; pick one consistently:
- **px-grid convention:** `1 px = 9525 EMU` (i.e. 96 px/in). `9525 × 1280 =
  12,192,000 EMU`.
- **inch-exact convention:** `1 px = (13.333/1280) × 914400 ≈ 9524.77 EMU`.

### §4.2 Canvas (16:9) — width is a flagged nuance
| Declared form | Exact EMU |
|---|---|
| `13.333in` (used by px-grid math) | **12,191,695 EMU** |
| `13.33in` (literal handoff-config value) | **12,188,952 EMU** |
| `1280 px × 9525` (px-grid) | **12,192,000 EMU** |
| Height `7.5in` | `6,858,000 EMU` |

> **Flag:** Do **not** state "13.333in = 12,192,000 EMU." 12,192,000 is the
> *px-grid* figure and is only internally consistent if you adopt `1px = 9525
> EMU` exactly. The handoff config literally declares **`13.33in`**
> (= 12,188,952 EMU). The 0.003in difference is real; use one convention end to
> end and note which.

### §4.3 % → EMU
`pct_of_width × 12,191,695` (13.333in basis) — but see the calc rows below for
exact values.

### §4.4 Worked `calc()` examples (corrected)
| Field | CSS | Inches | EMU |
|---|---|---|---|
| `two_column` right_column x | `calc(52% + 0px)` | `0.52×13.333 = 6.93316in` | **6,339,682** |
| `content_slide` content_area w | `calc(100% − 80px)` | `12.4997in` | **11,429,714** |

> **Flag:** earlier drafts rounded these to clean inches (6.9343in /
> 12.500in → 6,340,800 / 11,430,000). Those are the *rounded-inch* EMU, not the
> calc-exact EMU. Use the exact values above; 11,430,000 EMU = exactly 12.5in,
> which the calc does not produce.

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
- **Castro** `stat_callout` → `stat_number`: **64pt**, family `Arial Black`,
  weight 900, color `#F5C518` — source `castro/layouts.json` (lines 219–224).
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
All hexes match `validation/stanford-uit-branding.json` and
`mission/theme.json` `brand_colors`. Prohibited-color set matches
`validation/brand-compliance.json`:
`prohibited_colors.low_contrast = ["#CCCCCC", "#EEEEEE"]` (line 30).

---

## §7. Asset-integrity remediation (core of this doc — all defects TRUE)

These tables map **referenced-but-missing** assets to the **real on-disk** file
to substitute.

### §7.1 Logos — referenced `.svg` do not exist; real files are PNGs
The 6 logo `file_path` entries are all `.svg` and **nonexistent**. The real
files are 6 PNGs. Substitution:

| Referenced (missing) | Real on-disk file |
|---|---|
| `*_stacked.svg` | `*_vertical.png` |
| `*_cardinal.svg` | `*_cardinal-black.png` |
| (other 4 `.svg` refs) | corresponding `.png` (vertical/cardinal mapping) |

Rule: **stacked ↔ vertical**, **`*_cardinal` ↔ `*_cardinal-black.png`**.

### §7.2 Fonts — referenced families/extensions do not exist
`shared-assets/config/font-mappings.json` references
`SourceSansPro-*.{woff2,woff,ttf}` and `Monaco.{ttf,otf}` — **none exist** on
disk. Disk has only `SourceSans3-*.woff2`. Substitute `SourceSansPro-*` →
`SourceSans3-*.woff2`; there is **no Monaco** asset — fall back to a system
monospace or drop monospace usage.

### §7.3 Icons — referenced but absent
`branding.json` references `icons/check.svg`, `icons/wrench.svg`,
`icons/alert.svg` — all **absent**. The **service icons** *do* exist as SVG at
their exact cited paths. Either supply the three missing icons or remove their
references.

---

## §8. Compliance evaluator (runnable, cited to `brand-compliance.json`)

### §8.1 Prohibited-color check
Flag any element color in `prohibited_colors.low_contrast`
(`#CCCCCC`, `#EEEEEE`).

> **Flag (see §8.4):** Castro uses `#CCCCCC` pervasively (theme `body_font`,
> layouts content_area/subtitle/stat_label, master-slides, title_slide
> subtitle). A literal prohibited-color check therefore flags **every Castro
> content slide**. Reconcile before enforcing.

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
2. **Logo minimum size:** `branding.json` says 2in/1.5in; `brand-compliance.json`
   says 1in.
3. **Cool Grey:** `branding.json` `cool_grey = #4D4F53`; Stanford official and
   `mission/theme.json` `text_muted = #53565A`. Both hexes confirmed present in
   their files; they disagree.
4. **Calibri** is not in `approved_fonts` (yet may appear as a fallback).
5. **Castro stat font size:** theme.json 72pt vs layouts.json 64pt (§5.1).
6. **Castro `#CCCCCC` body text** is on the prohibited `low_contrast` list yet is
   the documented Castro body color (§8.1). `castro/metadata.json` even
   instructs "Body text should use #CCCCCC or lighter" — directly contradicting
   the prohibited list. Decide policy before enforcing the §8.1 check.

---

## §9. Edge cases & fallbacks
- Missing asset → apply §7 substitution; if no substitute exists (Monaco,
  missing icons), degrade gracefully and log.
- Selection uncertain → sunset (§3 Step 1.5).
- Unit convention → pick px-grid **or** inch-exact (§4.2) and stay consistent.
- Prohibited-color / placement / size conflicts (§8.4) → surface to the caller;
  do not auto-resolve.
