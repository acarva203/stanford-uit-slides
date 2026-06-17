---
name: stanford-uit-slides
description: Creates Stanford University IT technology branded presentations and slide decks . Use when user asks to create a "“slides”, “slide deck”, “presentation”, and “deck". This skill MUST prompt the user for permission before executing and ask if they are sure they want to use this stanford-uit-slides skill or the  pptx skill.

---


# stanford-uit-slides
## Skill.md Structure Aligned with Progressive Disclosure

### **Core Skill Definition (Essential Layer)**

# Stanford UIT Presentations

Creates brand-compliant PowerPoint presentations for Stanford University IT using official templates and automated Stanford identity guidelines.

## Core Capability
Generates professional IT presentations by selecting appropriate templates (Sunset, Noe, Castro, Mission), applying Stanford/UIT branding, and structuring content for technical audiences.

**Dependencies**: Requires `pptx` skill for PowerPoint generation.

## Basic Usage
- "Create a technical briefing for the cloud migration project"
- "Build security training slides for staff workshop" 
- "Generate executive presentation for CIO budget review"
- "Make department vision slides for strategic planning"

The skill automatically selects templates and applies branding based on your content and audience.


### **Template Selection Guide (Contextual Layer)**
## Template Options

### When Templates Are Auto-Selected
The skill chooses based on keywords and context:
- **Technical content** ("data", "metrics", "analysis") → **Noe** template
- **Executive content** ("leadership", "strategic", "CIO") → **Castro** template  
- **Vision content** ("mission", "culture", "storytelling") → **Mission** template
- **Standard content** ("update", "briefing", "routine") → **Sunset** template

### Manual Template Selection
Override automatic selection by specifying:
- "Use Castro template for this network security briefing"
- "Create this with the Mission template style"
- "Generate using Noe template for detailed technical content"

## Template Characteristics
- **Sunset**: Standard professional (16:9) - general purpose
- **Noe**: Minimal, data-focused (16:9) - technical audiences
- **Castro**: Bold, high-impact (16:9) - executive audiences  
- **Mission**: Narrative storytelling (16:10) - strategic/cultural content


### **Content Types & Structure (Functional Layer)**

## Supported Presentation Types

### Technical Briefing
**Auto-structure**: Executive Summary → Current State → Proposed Changes → Timeline → Next Steps
**Best for**: Project updates, system reviews, technical proposals
**Templates**: Sunset (standard), Castro (executive audience)

### Security Training  
**Auto-structure**: Objectives → Policies → Best Practices → Demo → Assessment → Resources
**Best for**: Staff education, awareness training, policy updates
**Templates**: Mission (engaging), Noe (detailed)

### Service Update
**Auto-structure**: Summary → Impact → Timeline → User Actions → Support
**Best for**: Maintenance announcements, service changes, outage communications
**Templates**: Sunset (standard), Noe (technical details)

### Project Proposal
**Auto-structure**: Problem → Solution → Implementation → Budget → Timeline
**Best for**: Initiative pitches, resource requests, strategic proposals  
**Templates**: Castro (executive), Mission (strategic vision)

### Incident Report
**Auto-structure**: Summary → Timeline → Impact → Root Cause → Prevention
**Best for**: Post-mortems, security incidents, service disruptions
**Templates**: Noe (technical), Sunset (general audience)

## Content Customization
Specify audience, technical depth, or emphasis:
- "Executive-level cloud migration briefing"
- "Detailed technical analysis of network performance" 
- "High-impact presentation for board strategic review"


### **Brand Standards & Compliance (Implementation Layer)**

## Stanford Brand Compliance

### Automatic Brand Application
- **Stanford Cardinal Red** (#8C1515) and approved color palette
- **Official Stanford wordmarks** with University IT identification
- **Source Sans Pro typography** (Mission template uses Georgia headlines)
- **Proper spacing, sizing, and layout** per Stanford Identity Guidelines

### UIT-Specific Elements
- **Department identification**: "Stanford University IT" branding
- **Technical status indicators**: Green (operational), Orange (maintenance), Red (incident)
- **Confidentiality markings**: Standard vs. "Internal Use Only" footers
- **IT service iconography**: Email, WiFi, security, infrastructure symbols

### Quality Validation
- Real-time brand guideline compliance checking
- Color contrast validation for accessibility (4.5:1 minimum)
- Logo placement and clear space verification
- Typography consistency across all slides

## Brand Compliance Score
Each presentation receives automatic validation:
- **95+ points**: Excellent Stanford brand compliance
- **85+ points**: Meets minimum brand standards  
- **<85 points**: Requires manual review or correction


### **Advanced Features & Integration (Technical Layer)**
## Technical Integration

### Skill Workflow

User Request → Template Selection → Content Structuring → Brand Application → pptx Generation


### Performance Characteristics
- **Template loading**: ~10ms (pre-processed JSON configs)
- **Brand application**: Real-time validation during generation
- **File generation**: Integrated with `pptx` skill for PowerPoint output
- **Asset management**: Cached logos, fonts, and configurations

### Advanced Customization

#### Template-Specific Features
- **Mission template**: 16:10 aspect ratio, expanded color palette, Georgia headlines
- **Castro template**: Bold typography emphasis, high-impact layouts
- **Noe template**: Minimal design, maximum content space, data-optimized
- **Sunset template**: Balanced professional design, versatile layouts

#### Audience Adaptations
- **Executive audiences**: Prefer Castro/Mission, avoid technical details
- **Technical staff**: Prefer Noe/Sunset, include detailed information
- **Mixed audiences**: Default to Sunset, balanced content depth
- **External stakeholders**: Prefer Mission/Castro, emphasize Stanford branding

### Asset Management
- **Logo files**: 6 variations (horizontal/vertical × black/cardinal/white)
- **Font files**: Source Sans Pro family, Monaco monospace, fallback sequences
- **Color configurations**: Stanford brand + UIT technical status colors
- **Icon library**: Technical services, security, infrastructure categories

## Error Handling & Fallbacks
- **Template selection uncertainty**: Defaults to Sunset template
- **Font unavailability**: Automatic fallback to Arial/Helvetica with preserved styling
- **Brand compliance failures**: Flags for manual review or blocks generation
- **Asset loading issues**: Graceful degradation with core Stanford branding



