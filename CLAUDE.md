# Mintlify documentation for qrtub

## Working relationship
- You can push back on ideas — this can lead to better documentation. Cite sources and explain your reasoning when you do so
- ALWAYS ask for clarification rather than making assumptions
- NEVER lie, guess, or make up information

## Project context
- Format: MDX files with YAML frontmatter
- Config: docs.json for navigation, theme, settings
- Components: Avoid using Mintlify components where possible because this content will likely be migrated to next.js

## Content strategy
- Document just enough for user success - not too much, not too little
- Prioritize accuracy and usability of information
- Make content evergreen when possible
- Search for existing information before adding new content. Avoid duplication unless it is done for a strategic reason
- Check existing patterns for consistency
- Start by making the smallest reasonable changes

## Frontmatter requirements for pages
- title: Clear, descriptive page title
- description: Concise summary for SEO/navigation

## Writing standards
- Second-person voice ("you")
- Prerequisites at start of procedural content
- Test all code examples before publishing
- Match style and formatting of existing pages
- Include both basic and advanced use cases
- Language tags on all code blocks
- Alt text on all images
- Relative paths for internal links

## Git workflow
- NEVER use --no-verify when committing
- Ask how to handle uncommitted changes before starting
- Create a new branch when no clear branch exists for changes
- Commit frequently throughout development
- NEVER skip or disable pre-commit hooks

## Do not
- Skip frontmatter on any MDX file
- Use absolute URLs for internal links
- Include untested code examples
- Make assumptions - always ask for clarification

---

## Brand, voice & glossary

These live in the app repo as the single source of truth. Reference them for all content decisions.

- **Brand & voice:** `c:/dev/qrtub/qrtub/BRAND.md` — product definition, tone, writing rules, audiences, positioning
- **Glossary:** `c:/dev/qrtub/qrtub/GLOSSARY.md` — canonical terms and what NOT to call things

When writing docs content: check the glossary first for correct terminology, then BRAND.md for tone and accuracy constraints (especially the TRUE/FALSE claims lists).

---

# DOCS CONTENT ARCHITECTURE

## Site Structure

```
qrtub.com
├── / (Homepage)
├── /getting-started (New user onboarding)
├── /how-it-works
├── /features
│   ├── /features/profile-pages
│   ├── /features/bulk-deployment
│   ├── /features/media-management
│   └── /features/multi-destination
├── /use-cases
│   ├── /use-cases/multi-system-integration
│   ├── /use-cases/audience-routing
│   ├── /use-cases/vendor-flexibility
│   └── /use-cases/print-before-link
├── /industries
│   ├── /industries/marine
│   ├── /industries/construction
│   ├── /industries/equipment-hire
│   └── /industries/lifesaving-emergency
├── /help
│   ├── /help/account
│   ├── /help/links
│   ├── /help/profile-pages
│   └── /help/media
├── /integrations
│   ├── /integrations/safetyculture
│   ├── /integrations/cmms-systems
│   └── /integrations/[others]
├── /blog
├── /beta
├── /about
└── /contact
```

## Page Types & Purposes

| Page Type | Purpose | Primary CTA | Tone |
|-----------|---------|-------------|------|
| Homepage | Introduce value prop, route to paths | Join BETA | Confident, clear |
| Feature pages | Explain capabilities in depth | Try in BETA | Practical, detailed |
| Use case pages | Show problem→solution stories | Join BETA | Problem-focused |
| Industry pages | Speak to specific verticals | Join BETA for [Industry] | Industry-aware |
| Documentation | Enable successful usage | N/A (utility) | Technical, clear |
| Blog | Educate, build authority, SEO | Soft CTAs | Educational |
| BETA page | Convert interested visitors | Sign Up | Direct, clear |

## CTA Strategy

**Primary (conversion):** "Join the BETA Program" / "Get BETA Access" / "Join BETA for [Industry]"
**Secondary (consideration):** "See How It Works" / "View Use Cases" / "Explore Features"
**Tertiary (education):** "Read the Guide" / "Learn More" / "View Documentation"

---

# PROMPTING TEMPLATES

Ready-to-use templates for content generation. Always read BRAND.md and GLOSSARY.md before using these.

## Homepage Hero Section

```
TASK: Write a homepage hero section for qrtub.

CONSTRAINTS:
- Headline: 6 words or fewer
- Subheadline: 15-25 words, problem-focused
- Body: 2-3 sentences explaining core value
- Primary CTA: "Join the BETA Program"
- Secondary CTA: "See How It Works"

VOICE: Professional but approachable. Lead with pain point.

OUTPUT FORMAT:
## Headline
[headline]

## Subheadline
[subheadline]

## Body
[2-3 sentences]

## CTAs
- Primary: [CTA text]
- Secondary: [CTA text]
```

## Feature Page Section

```
TASK: Write a feature description for [FEATURE NAME].

STRUCTURE:
1. What it is (1 sentence)
2. The problem it solves (2-3 sentences)
3. How it works (2-3 sentences)
4. Example scenario (3-4 sentences)
5. Key benefits (3 bullet points, each 10-15 words)

CONSTRAINTS:
- Do not claim capabilities beyond those listed in BRAND.md feature status
- Use glossary terms consistently
- Include specific, concrete details
- End with clear next step

VOICE: Practical, specific, confident.
```

## Industry Landing Page

```
TASK: Write a landing page for qrtub targeting [INDUSTRY].

INDUSTRY CONTEXT:
- Key items managed: [list]
- Key pain points: [list]
- Common software used: [list]
- Compliance/regulatory factors: [list]

STRUCTURE:
1. Hero: Industry-specific headline + subheadline
2. Problem section: 3 industry pain points
3. Solution section: How qrtub addresses each
4. Use case: One detailed scenario
5. Integration callout: Mention industry-relevant tools
6. CTA: "Join BETA for [Industry]"

CONSTRAINTS:
- Use industry terminology
- Reference real tools they use
- Do not claim qrtub replaces those tools
- Keep technical accuracy
```

## Use Case Page

```
TASK: Write a use case page for "[USE CASE NAME]"

USE CASE FACTS:
- Problem: [specific problem]
- Without qrtub: [what happens]
- With qrtub: [what changes]
- Key features used: [list]

STRUCTURE:
1. Problem statement (2-3 sentences, specific scenario)
2. The old way (3-4 sentences, pain points)
3. The qrtub way (3-4 sentences, solution)
4. Step-by-step workflow (4-6 numbered steps)
5. Outcome (2-3 sentences, concrete benefits)
6. Related use cases (2-3 links)
7. CTA

CONSTRAINTS:
- Be specific — use numbers, scenarios
- Don't invent customer quotes
- Stick to available features only

VOICE: Problem-focused, practical.
```

## FAQ Answer

```
TASK: Write an FAQ answer for: "[QUESTION]"

CONSTRAINTS:
- First sentence directly answers the question
- 2-4 sentences total
- Include "Learn more" link if relevant
- If touching on planned features, be explicit about status

FORMAT:
**Q: [Question]**
A: [Answer]
```

## Blog Post

```
TASK: Write a blog post titled "[TITLE]"

TOPIC: [description]
TARGET AUDIENCE: [simple users / sophisticated users / both]
GOAL: [educate / convert / SEO]

STRUCTURE:
1. Hook (problem or question they recognize)
2. Context (why this matters now)
3. Main content (3-5 sections)
4. qrtub connection (how qrtub relates — subtle, not salesy)
5. Takeaway (actionable conclusion)
6. CTA (soft — "Learn more" not "Buy now")

CONSTRAINTS:
- Educational tone, not promotional
- Specific examples and scenarios
- Link to qrtub features only where genuinely relevant
- 800-1200 words
```

## Comparison Content

```
TASK: Write a comparison between qrtub and [ALTERNATIVE TYPE].

ALTERNATIVE: [QR generators / link shorteners / asset management software / bio link tools]

STRUCTURE:
1. What [alternative] does well (fair, accurate)
2. Where it falls short for physical deployments (specific)
3. How qrtub differs (factual, not disparaging)
4. When to use which (honest guidance)
5. Summary table

CONSTRAINTS:
- Be fair to alternatives — don't strawman
- Focus on use case fit, not "better/worse"
- Acknowledge where alternatives might be better choice
- Stay factual — no invented limitations

VOICE: Objective, helpful, confident.
```

---

# USE CASES LIBRARY

## UC-001: Multi-System Integration
**Tagline:** "One QR code for every system"

**Problem:** Equipment needs QR codes for multiple software systems — inspection apps, maintenance tracking, compliance platforms, customer portals. Each system generates its own codes.

**Without qrtub:** Multiple QR codes per Item (3-5 stickers common), visual clutter, user confusion, management overhead.

**With qrtub:** One QR code per Item, Profile Page with Destinations for each system, users self-select the right function.

**Key features:** Profile Pages, multiple Destinations, URL Templates

## UC-002: Audience Routing
**Tagline:** "Right information for the right person"

**Problem:** The same physical item needs to serve different audiences — staff need operational tools, customers need support info, managers need compliance docs.

**Without qrtub:** Different codes for different audiences, or everyone sees everything.

**With qrtub:** One Profile Page, Destinations visible to the relevant audience. Advanced: Conditional Visibility based on Item fields or device type.

**Key features:** Profile Pages, multiple Destinations, Conditional Visibility (advanced)

## UC-003: Vendor Flexibility / Future-Proofing
**Tagline:** "Change your mind without reprinting"

**Problem:** Switching software vendors means reprinting every QR code if they're statically linked to the old system's URL.

**Without qrtub:** Reprint costs, downtime, sticker removal and replacement across the fleet.

**With qrtub:** Update the Destination URL in qrtub. Physical codes unchanged.

**Key features:** Destination updates, Direct Mode or Profile Mode

## UC-004: Print-Before-Link Workflow
**Tagline:** "Print first, connect when ready"

**Problem:** Professional bulk printing requires lead time, but equipment details may not be finalized yet.

**Without qrtub:** Wait until system is ready (delays), or print individual codes one at a time (expensive).

**With qrtub:** Generate Links in bulk, get QR codes printed, connect them to Items during field installation.

**Key features:** Bulk Link generation, print-before-link workflow, Link/Item decoupling
