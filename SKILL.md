---
name: stanford-uit-slides
description: Creates Stanford University IT branded presentations and slide decks. Use when the user asks to create a "slide deck", "presentation", "slides", or "deck". This skill MUST prompt the user for permission before executing and ask whether they want to use this stanford-uit-slides skill or the pptx skill.

---

# Stanford UIT Presentations

Creates brand-compliant PowerPoint decks for Stanford University IT from official
templates, applying Stanford identity guidelines automatically.

**This file is the *what*. The build procedure (the *how*) lives in
[`skill-complementary.md`](skill-complementary.md)** — exact paths, CSS→EMU unit
math, per-slide python-pptx recipes, branding application, and the compliance
evaluator. Read this first for selection, then that to generate.

## Templates

| Template | Style | Aspect | Best for |
|---|---|---|---|
| **Sunset** | Balanced, professional | 16:9 | General purpose; the default |
| **Noe** | Minimal, data-dense | 16:9 | Technical / detailed audiences |
| **Castro** | Bold, high-impact | 16:9 | Executive / leadership |
| **Mission** | Narrative, Georgia headlines | 16:10 | Strategic / cultural content |

**Auto-selection** keys off the brief (full logic in
`integration/template-selector.json`): *data / metrics / analysis* → Noe;
*leadership / strategic / CIO* → Castro; *mission / culture / storytelling* →
Mission; *update / briefing / routine* → Sunset. When uncertain, default to
**Sunset**.

**Manual override:** name the template, e.g. "Use Castro for this security
briefing." For executive audiences prefer Castro/Mission; for technical staff,
Noe/Sunset.

## Presentation types

Each type auto-structures its sections:

| Type | Section flow | Suggested templates |
|---|---|---|
| Technical Briefing | Exec Summary → Current State → Proposed Changes → Timeline → Next Steps | Sunset, Castro |
| Security Training | Objectives → Policies → Best Practices → Demo → Assessment → Resources | Mission, Noe |
| Service Update | Summary → Impact → Timeline → User Actions → Support | Sunset, Noe |
| Project Proposal | Problem → Solution → Implementation → Budget → Timeline | Castro, Mission |
| Incident Report | Summary → Timeline → Impact → Root Cause → Prevention | Noe, Sunset |

Specify audience, technical depth, or emphasis to tune content (e.g.
"executive-level cloud migration briefing").

## Brand standards

Applied automatically; rules and scoring in `validation/brand-compliance.json`,
values in `shared-assets/stanford-uit-branding.json`.

- **Color:** Stanford Cardinal `#8C1515` + approved palette; status colors —
  green (operational), orange (maintenance), red (incident).
- **Typography:** Source Sans 3 (Mission uses Georgia headlines); Roboto for code.
- **Logos:** official Stanford | University IT wordmarks, 6 variants
  (horizontal/vertical × black/cardinal/white).
- **Markings:** standard vs. "Internal Use Only" footers; clear-space and
  minimum-size rules enforced.
- **Accessibility:** text contrast checked to a 4.5:1 minimum.

**Compliance score:** ≥95 excellent · ≥85 passing · <85 requires review or is
blocked.

## Workflow

```
Request → Template Selection → Content Structuring → Brand Application → pptx Generation
```

Each stage is specified in [`skill-complementary.md`](skill-complementary.md):
selection (§3), unit translation (§4), per-slide recipes (§5), branding (§6),
asset substitutions (§7), compliance scoring (§8).

## Fallbacks

- **Uncertain template** → Sunset.
- **Missing font** → fall back to Arial/Helvetica, styling preserved.
- **Missing asset** → graceful degradation with core Stanford branding (see
  §7 substitutions).
- **Compliance failure** → flag for review, or block generation on critical.

