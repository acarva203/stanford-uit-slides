# Template Extraction Documentation

## Overview
This document details the process of converting Stanford UIT PowerPoint templates into JSON configurations for the `stanford-uit-slides` skill.

**Extraction Date**: June 17, 2026  
**Extracted By**: Aishwari  
**Source Templates**: 4 official Stanford UIT PowerPoint templates  
**Target Format**: JSON configuration files for automated presentation generation

---

## Source Materials

### Template Files
- `Powerpoint_Slide_Template_A-Sunset.pptx`
- `Powerpoint_Slide_Template_B-Noe.pptx` 
- `Powerpoint_Slide_Template_C-Castro.pptx`
- `Powerpoint_Slide_Template_D-Mission.pptx`

### Supporting Materials
- Stanford Brand Guidelines (2024)
- University IT Logo Variations:
  - `University-IT_wordmark_horizontal_black.png`
  - `University-IT_wordmark_horizontal_cardinal-black.png`
  - `University-IT_wordmark_horizontal_white.png`
  - `University-IT_wordmark_vertical_black.png`
  - `University-IT_wordmark_vertical_cardinal-black.png`
  - `University-IT_wordmark_vertical_white.png`

---

## Extraction Methodology

### AI-Assisted Analysis Process
Essentially, I used Claude to extract and discern necessary markers and features to be separated and added into the JSON configuration files.

**Process Overview**:
1. **Template Upload**: Provided PowerPoint files to Claude for analysis
2. **Systematic Extraction**: Claude analyzed visual properties, layouts, and design patterns
3. **JSON Structure Generation**: Converted findings into structured configuration files
4. **Cross-Template Comparison**: Identified common elements and unique characteristics
5. **Validation**: Verified extracted data against original templates

### Specific Extraction Tasks

#### **Visual Properties Extracted**
- **Color Palettes**: Hex codes for all colors used in templates
- **Typography**: Font families, weights, sizes, and hierarchy
- **Logo Specifications**: Placement, sizing, and usage guidelines  
- **Layout Dimensions**: Slide sizes, margins, content areas
- **Styling Elements**: Backgrounds, borders, spacing patterns

#### **Template Categorization**
Claude identified distinct template personalities:
- **Sunset**: Standard professional, general-purpose
- **Noe**: Minimal, content-focused, data-heavy presentations
- **Castro**: Bold, high-impact, executive-level
- **Mission**: Narrative storytelling, inspirational content (16:10 aspect ratio)

#### **JSON Structure Design**
Organized extracted data into:
- `template-configs/[template-name]/theme.json`
- `template-configs/[template-name]/layouts.json`
- `template-configs/[template-name]/master-slides.json`
- `template-configs/[template-name]/metadata.json`

---

## Key Findings & Decisions

### Template Characteristics Discovered

#### **Sunset Template**
- **Aspect Ratio**: 16:9 standard
- **Primary Use**: General business presentations
- **Color Scheme**: Stanford Cardinal with professional greys
- **Typography**: Source Sans Pro family, balanced weights

#### **Noe Template** 
- **Aspect Ratio**: 16:9 standard
- **Primary Use**: Technical content, data visualization
- **Color Scheme**: Minimal palette, high contrast
- **Typography**: Source Sans Pro with lighter weights

#### **Castro Template**
- **Aspect Ratio**: 16:9 standard  
- **Primary Use**: Executive presentations, high-impact content
- **Color Scheme**: Bold Stanford Cardinal emphasis
- **Typography**: Source Sans Pro with heavier weights

#### **Mission Template**
- **Aspect Ratio**: 16:10 (unique)
- **Primary Use**: Strategic vision, storytelling presentations
- **Color Scheme**: Expanded Stanford brand palette with section dividers
- **Typography**: Georgia for headlines, Calibri for body (serif/sans mix)

### Design Patterns Identified
1. **Consistent Branding**: All templates maintain Stanford identity compliance
2. **Hierarchical Typography**: Clear heading/body/caption distinction
3. **Flexible Layouts**: Multiple slide layout options per template
4. **Logo Integration**: Standardized University IT wordmark placement
5. **Color Coding**: Template-specific color usage patterns

### Assumptions & Interpretations Made

#### **Template Selection Logic**
- Assigned use cases based on visual characteristics
- Mapped content types to appropriate templates
- Defined audience preferences for each style

#### **Technical Specifications**
- Standardized measurement units (points for fonts, pixels for layouts)
- Created fallback sequences for fonts and colors
- Established cross-platform compatibility requirements

#### **Integration Requirements**
- Designed JSON structure for seamless handoff to `pptx` skill
- Created validation rules for brand compliance
- Implemented performance optimization strategies

---

## Validation Process

### Cross-Reference Checks
- Verified color codes against Stanford brand guidelines
- Confirmed font specifications match template usage
- Validated layout measurements through multiple template examples

### Edge Cases Identified
- **Mission Template**: Unique 16:10 aspect ratio requires special handling
- **Font Fallbacks**: Monaco monospace font may not be available on all systems
- **Logo Variations**: Different templates prefer different logo orientations

### Quality Assurance
- Tested JSON configurations against original template designs
- Verified template selection logic with sample presentation requests
- Confirmed brand compliance across all generated outputs

---

## Future Maintenance Notes

### Update Triggers
- Stanford brand guideline changes
- New template releases from University IT
- Font availability changes
- Performance optimization opportunities

### Maintenance Process
1. Re-analyze updated source templates
2. Update relevant JSON configurations
3. Test changes against existing presentation generation
4. Update validation rules if needed
5. Document changes in this file

### Known Limitations
- Extraction process cannot capture subjective design intent
- Some advanced PowerPoint features may not translate to JSON
- Template updates require manual re-analysis
- Font licensing considerations not captured in configurations

---

## File Generation Summary

### Created Configurations
- **4 template directories**: sunset/, noe/, castro/, mission/
- **16 JSON files total**: 4 files per template (theme, layouts, master-slides, metadata)
- **1 unified branding file**: stanford-uit-branding.json
- **1 font mapping file**: font-mappings.json
- **2 integration files**: template-selector.json, pptx-handoff-config.json

### Asset Organization
- **6 logo files**: Horizontal and vertical wordmarks in 3 color variations
- **Font files**: Source Sans Pro weights, Monaco monospace
- **Icon library**: Technical services, security, infrastructure categories

This extraction process successfully converted 4 PowerPoint templates into a comprehensive JSON-based configuration system for automated Stanford UIT presentation generation.