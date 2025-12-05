# Mintlify documentation for qrtub

## Working relationship
- You can push back on ideas-this can lead to better documentation. Cite sources and explain your reasoning when you do so
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

# qrtub Content Development Brief
## AI-Era Edition — For LLM Content Generation & Discovery

**Version:** 2.0  
**Purpose:** Source of truth for AI-generated website content and AI search discovery  
**Primary Consumer:** Claude Code (and other LLMs)  
**Status:** BETA Phase

qrtub is a product built and owned by Australian company Teralis Pty Ltd

qrtub only contact email is hi@qrtub.com

---

# PART 1: SOURCE OF TRUTH

This section contains canonical facts. AI systems should treat these as authoritative and not embellish, speculate, or contradict.

---

## 1.1 Product Definition

### Canonical Single-Sentence Definition
> **qrtub is a QR code management platform that decouples physical QR codes from their digital Destinations, enabling bulk deployment, flexible linking, and multi-system integration through professional Profile Pages.**

### Canonical Two-Sentence Definition
> **qrtub manages the entire lifecycle of physical QR codes—from bulk generation and professional printing to field deployment and ongoing updates. One QR code becomes a smart hub, connecting to multiple Destinations, serving different audiences, and changing where it points without reprinting.**

### Canonical Paragraph Definition
> **qrtub solves the operational reality of physical QR code deployments. Unlike simple QR generators that assume you'll print one code at a time and never change your mind, qrtub is built for organizations deploying QR codes at scale. Generate Links in bulk for professional printing, apply them to physical items before your systems are ready, connect them to Destinations when convenient, and update those Destinations forever—without reprinting. Each QR code can become a Profile Page with multiple Destinations for different audiences, integrating with your existing software tools rather than replacing them.**

---

## 1.2 Entity Model

```
ACCOUNT (top-level container)
├── LINKS (account-level, not tub-level)
│   ├── Random: qrtub.com/r/x5fgd
│   ├── ID-based: qrtub.com/cra001
│   └── Custom: qrtub.com/mylink
│
├── MEDIA (physical materials displaying QR codes) [Basic: Available | Advanced: Planned]
│   ├── The physical sticker, plaque, NFC chip, sign, billboard, or printed material encoding a Link
│   ├── Has its own lifecycle independent of Link and Item
│   ├── Linked to a Link
│   ├── Created from MEDIA TEMPLATES (design templates)
│   └── Grouped into MEDIA BATCHES (production runs)
│
├── MEDIA TEMPLATES (reusable designs) [Planned]
│   ├── System-provided templates (standard sticker sizes, sign formats)
│   └── Customer-saved templates (custom designs)
│
├── MEDIA BATCHES (production runs) [Planned]
│   ├── Print/production date, material type, source/partner
│   ├── Quantity tracking
│   └── One Link can appear across multiple Media Batches (reprints, different formats)
│
├── TUBS (category workspaces)
│   ├── Define custom fields
│   ├── Define profile page templates
│   └── Contain ITEMS
│       ├── Standard fields: Number, Name, Description, Group, Status, Tags, Date, Image
│       ├── System fields: created_at, updated_at, created_by, updated_by
│       ├── Custom fields: Defined by tub
│       └── PROFILE PAGE (1:1 with item)
│           └── DESTINATIONS (buttons/options for users to select)
```

### The Three-Entity Distinction

Most people think generating a QR code pattern is all there is—but qrtub recognizes that physical QR code deployments involve three distinct things:

| Entity | What It Is | Example |
|--------|-----------|---------|
| Item | The thing being represented | An excavator, a fire extinguisher, a meeting room |
| Link | The qrtub-managed URL (the digital pattern) | `qrtub.com/r/x5fgd` — where the QR code resolves to |
| Media | The physical material the QR code is displayed on | A vinyl sticker, a metal plaque, a billboard, a real estate sign, a printed ad, an NFC chip |

**Terminology note:** In general descriptions, "QR code" is natural and ubiquitous. "Media" or "physical media" is qrtub's term for the actual physical thing the QR code is printed/displayed on—managing inventory, costs, production runs, and replacements.

**Why this matters:**
- A metal plaque costs $50 and lasts 10 years — it's infrastructure worth tracking
- A billboard costs $5,000 — that's a major asset
- The same Link might be reproduced on new Media if the original is damaged
- Media might be moved between Items (equipment gets replaced, sign stays)
- Different Media types have different durability, cost, and use cases (vinyl sticker vs engraved plaque vs billboard)
- Organizations need to manage their physical QR code infrastructure inventory
- Media Batches let you track production runs separately from individual Media items

### How Links Flow

```
QR CODE (what users scan) 
  └── encodes → LINK (qrtub URL)
                  ├── Direct Mode: → DESTINATION (single, immediate)
                  └── Profile Mode: → Profile Page
                                      ├── DESTINATION
                                      ├── DESTINATION
                                      └── DESTINATION
```

A **Destination** can be:
- An external URL (e.g., SafetyCulture inspection)
- A qrtub Doc Library (planned)
- A qrtub Form (planned)
- A payment flow (planned)
- Future features

### Media Management

| Capability | Status | Description |
|------------|--------|-------------|
| Basic Media tracking | Available | Record Media type (sticker, sign, plaque, etc.), associate with Links |
| Media as distinct entity | Planned | Full lifecycle management of physical Media |
| Media inventory management | Planned | Track stock, orders, costs, conditions |
| Media replacement workflow | Planned | Link new Media to existing Link when original is damaged |
| Media location tracking | Planned | Where is the physical Media installed vs. what Item it represents |
| Media Templates | Planned | Reusable design templates for producing Media |
| Media Batch management | Planned | Track production runs: date, type, quantity, source/partner |

### Key Architectural Facts

| Principle | Fact |
|-----------|------|
| Decoupling | Links and Items are independent entities. Links can exist before Items. Items can exist before Links are assigned. |
| Flexibility | Links can be Direct Mode (single Destination) OR Profile Mode (multiple Destinations via Profile Page). This can be changed at any time. |
| Scope | Links are account-level, not tub-level. One Link can be reassigned across Tubs. |
| Multiplicity | Multiple Links can point to one Item. One Link points to one destination (Item or direct Destination). |
| Physical Independence | Media (physical materials) are tracked separately from Links. Replace damaged Media without losing Link history. |

### Planned Architectural Capabilities (Not Yet Available)

| Capability | Description | Use Cases |
|------------|-------------|-----------|
| Granular Permissions & Cross-Account Sharing | Transfer ownership of Links and Items between accounts. Share access across teams/organizations with configurable permissions. | **Manufacturer → Buyer Transfer:** Equipment supplier prints branded QR codes in factory, transfers ownership to buyer who continues the asset lifecycle. **Cross-Rental:** Company A hires from Company B to on-hire to Company C; Link access transfers from B's system to A or C's system for duration of hire. **Multi-Party Access:** Maintenance contractors access client equipment records without full ownership transfer. |
| Media Batch Management & Templates | Track production runs of physical Media: print date, material type, quantity, source/partner. Reusable design templates for consistent production. | **Production Tracking:** "Media Batch #47: 500 weatherproof vinyl stickers, printed March 2025 by PrintCo." **Reprint Management:** Same Links printed across multiple Media Batches over time. **Cost Allocation:** Track Media production costs by Batch. **Template Library:** System-provided and customer-saved design templates for stickers, signs, plaques, billboards. |

---

## 1.3 Capability Boundaries

### What qrtub IS (Factual)

- A QR code management platform
- A deployment management system for physical QR codes
- An integration layer connecting physical items to multiple digital systems
- A profile page builder for multi-link landing pages
- A URL redirect manager with update-without-reprint capability

### What qrtub IS NOT (Explicit Boundaries)

| qrtub is NOT | Clarification |
|--------------|---------------|
| Asset management software | It integrates with asset management systems, doesn't replace them |
| Inspection software | It links to inspection tools like SafetyCulture, doesn't provide inspection features |
| A maintenance tracking system | It connects to CMMS systems, doesn't track maintenance itself |
| A compliance platform | It links to compliance tools, doesn't manage compliance |
| An e-commerce platform | Future payment features planned, but not core function |
| A link shortener | It manages physical deployments, not just digital link sharing |
| A social media bio tool | It's for physical items, not online profiles |

### Current Feature Status (BETA)

| Feature | Status |
|---------|--------|
| Link generation (bulk) | Available |
| Profile Pages with Destinations | Available |
| Direct Mode (single Destination) | Available |
| Tub-based organization | Available |
| Spreadsheet-style grid management | Available |
| URL Templates in Destinations | Available |
| Conditional visibility (CEL) | Available (advanced) |
| Import/export | Available |
| Basic Media tracking | Available |
| Advanced Media lifecycle management | Planned |
| Media Templates | Planned |
| Media Batch management | Planned |
| Cross-account transfer & sharing | Planned |
| Granular permissions | Planned |
| API access | Planned |
| Payment Destinations | Planned (6+ months) |
| Advanced analytics | Planned |

---

## 1.4 Technical Specifications

### Link URL Structures

| Type | Format | Example | Use Case |
|------|--------|---------|----------|
| Random | `qrtub.com/r/{5-char}` | `qrtub.com/r/x5fgd` | Default generation |
| ID-based | `qrtub.com/{id}` | `qrtub.com/cra001` | Sequential/branded |
| Custom | `qrtub.com/{custom}` | `qrtub.com/mylink` | Memorable URLs |
| Custom Domain | `{yourdomain}/{path}` | `my.link/cra001` | White-label (planned) |

### Profile Page Capabilities

| Capability | Description |
|------------|-------------|
| Multiple Destinations | Buttons that route to different endpoints |
| URL Templates | Construct Destination URLs using Item field values as placeholders: `app.com/inspect?id={assetID}`. When scanned, qrtub automatically fills in the Item's field values—enabling bulk deployment where each Item's QR code routes to a personalized URL without manual configuration. |
| Conditional Visibility (Advanced) | Show/hide Destinations based on conditions using CEL expressions. Premium feature for complex routing scenarios. |
| Branding | Custom colors, logos, descriptions |

#### Conditional Visibility (Advanced Feature)

**What it is:** Control which Destinations appear on Profile Pages based on Item field values or device characteristics using CEL (Common Expression Language) expressions.

**Who needs this:** Advanced users with complex routing requirements. Most users won't need conditional visibility—URL Templates and multiple Destinations handle the majority of use cases.

**Key use cases:**

1. **Tag-based routing:** Show specific Destinations only for tagged Items
   - Example: Show "Crane Pre-Start Inspection" only if `tag == "crane"`

2. **Equipment type-specific inspections:** Different inspection Destinations for different equipment types
   - Example: Show "Forklift Inspection" only if `equipmentType == "forklift"`

3. **Expiry warnings:** Display overdue/expiry banners based on date fields
   - Example: Show "Inspection Overdue" Destination if `inspectionDue` date has passed

4. **Device-specific Destinations:** Route mobile vs desktop users to different endpoints
   - Example: Show mobile app link only on mobile devices
   - Example: `device.isDesktop || (device.isIOS && device.browser != "safari")` — show desktop link OR show for iPhone users not using Safari (iOS blocks app deep links except in Safari)

**How it works:** Reference any Item field (standard, custom, or system fields) plus device detection fields in CEL expressions to control Destination visibility.

**Available for conditions:**
- **Item fields:** Any field from your Items (tags, status, equipmentType, inspectionDue, custom fields, etc.)
- **Device fields:** device.isDesktop, device.isIOS, device.browser, etc.

**CEL Standard:** Conditional visibility uses CEL (Common Expression Language), an industry-standard expression language. Complex expressions can be generated using AI tools.

**Example ChatGPT prompt for generating CEL expressions:**
```
I'm using qrtub Profile Pages with conditional visibility (CEL expressions).
I want to show a Destination called "Crane Inspection" only when:
- The Item has tag "crane" OR tag "heavy-equipment"
- AND the inspection due date (field: inspectionDue) is within the next 30 days

Generate a CEL expression for this condition.
Available fields: tag (string), inspectionDue (date), equipmentType (string)
Available operators: ==, !=, ||, &&, >, <
```

**When to use:** Only when URL Templates and basic multiple Destinations don't solve your routing needs. Most scenarios don't require conditional visibility.

### Distribution Models

| Model | How It Works | Use Case | Status |
|-------|--------------|----------|--------|
| Direct Generation | User creates Links in their account | Standard usage | Available |
| Pre-allocated | Partner assigns Links to specific account | Custom orders | Available |
| Claim-on-scan | Link claimed on first scan | Retail/off-the-shelf | Available |
| Transfer | Ownership moves between accounts | Manufacturer → Customer, Cross-rental | Planned |

---

## 1.5 Canonical Glossary

Use these terms consistently. Do not invent synonyms.

| Term | Definition | Usage Notes |
|------|------------|-------------|
| qrtub | The platform name. Always lowercase, one word. | NEVER: QRTub, QR Tub, Qrtub |
| Link | A qrtub-managed URL that resolves to a destination. Can be encoded on any medium (QR code, NFC, RFID, typed). | Use instead of: Access Link, Access URL, QR link, smart link |
| Destination | Where users are routed—an external URL, a qrtub doc library, a form, a payment flow, or future features. Also the UI element on Profile Pages. | Use instead of: Action Link, Destination Link, Link Card |
| Profile Page | Multi-link landing page associated with an Item. Contains one or more Destinations. | Use instead of: Hub page, landing page (alone), bio page |
| QR code | The digital pattern (scannable code) that encodes a Link. Most people think this is all there is—just generating the pattern. | Use in general product descriptions, marketing, discussing the technology. This is the natural, ubiquitous term. |
| Media | The physical material the QR code is displayed on—vinyl stickers, metal plaques, signs, billboards, real estate signs, printed ads, NFC chips, engraved monuments, etc. This is what costs money and needs lifecycle management. | Use when: tracking physical inventory, discussing production, managing replacements, referencing costs. Be explicit in marketing: "physical media" or "physical QR code media - stickers, signs, plaques" |
| Media Template | Reusable design template for producing Media. Can be system-provided or customer-saved. | Examples: "4x6 vinyl sticker template," "24x36 real estate sign template," "billboard template" |
| Media Batch | A production run of Media items produced together. Tracks print/production date, type, source/partner, quantity, costs. One Link may appear across multiple Media Batches (reprints, different formats). | Use instead of: Print run, Production run, Order (when referring to production group). Industry term used by print and signage partners. |
| Media Partners | Print shops, signage companies, engravers, and other producers who create physical Media for qrtub customers. | Industry-standard term. "Certified qrtub Media Partner" |
| Tub | Category-based workspace with custom fields and templates | Use instead of: Folder, category, group |
| Item | Individual entity being tracked (equipment, location, product) | Use instead of: Asset, object, thing |
| Direct Mode | Link bypasses Profile Page, goes straight to a single Destination | Use instead of: Simple redirect, forwarding |
| Profile Mode | Link opens a Profile Page where user selects from multiple Destinations | Use instead of: Landing mode |
| Print-before-link | Workflow: print Links on QR codes first, connect to Items later | Use instead of: Pre-print, advance printing |
| Claim-on-scan | First scan claims ownership of Link | Use instead of: Activation, registration |

### QR Code vs Media: When to Use Each

| Context | Use "QR code" | Use "Media" or "Physical Media" |
|---------|---------------|--------------------------------|
| Product descriptions | "Deploy QR codes in bulk" | |
| Marketing headlines | "One QR code for every system" | |
| General feature explanations | "Change where QR codes point" | |
| Homepage/landing pages | | "Track your physical QR code media - stickers, signs, plaques" (be explicit) |
| Physical asset tracking | | "Track your Media as inventory" |
| Production/batch management | | "Media from Batch #47" |
| Replacement workflows | | "Replace damaged Media" |
| Cost/inventory tracking | | "Media inventory management" |
| UI section labels | | "Media Management" or "Physical Media" |
| Store/purchasing context | | "Order 500 stickers" or "Order Media" |
| Partner relationships | | "Media Partners" |
| Design templates | | "Media Templates" |

**Rule of thumb:** Use "QR code" when talking about what users scan (the digital pattern). Use "Media" or "physical media" when talking about what organizations produce, track, manage, and inventory (the physical stickers, signs, plaques, billboards, etc.). In customer-facing marketing, be explicit: "physical QR code media like stickers, signs, and plaques."

---

# PART 2: AI-READY CONTENT BLOCKS

These are pre-approved statements that AI can extract, quote, or paraphrase with confidence.

---

## 2.1 Canonical Descriptions by Length

### One-liner (< 15 words)
> qrtub: Deploy QR codes in bulk, change Destinations without reprinting.

### Tagline
> Deploy easier. Do more.

### Elevator Pitch (30 seconds / ~75 words)
> Physical QR codes are permanent once you stick them on equipment—but your systems aren't. qrtub decouples your QR codes from their Destinations. Print professional codes in bulk before your systems are ready, connect them during installation, and update where they point forever. One QR code becomes a Profile Page connecting to multiple systems. When you switch software vendors in two years, update the Destination—don't reprint 500 stickers.

### Short Description (~50 words)
> qrtub manages physical QR code deployments from bulk printing to ongoing updates. Generate Links before Items exist in your system, print QR codes, apply them during field installation, and change Destinations without reprinting. Each QR code can become a multi-Destination Profile Page, connecting one physical item to multiple software systems.

### Medium Description (~100 words)
> qrtub is a QR code management platform built for the reality of physical deployments. Unlike simple QR generators, qrtub handles the full lifecycle: bulk generation for professional printing, flexible linking during field installation, and Destination updates without reprinting.
>
> Each QR code can work as a Direct Mode redirect or become a Profile Page with multiple Destinations—serving different information to different audiences from one code. Staff tap to open inspection apps; customers tap to see product info.
>
> qrtub integrates with your existing tools (SafetyCulture, maintenance systems, compliance platforms) rather than replacing them. It's the layer between your physical items and your digital systems.

---

## 2.2 Feature Descriptions (Canonical)

### Bulk Link Generation
> Generate hundreds of Links at once for professional QR code printing. Links exist independently—connect them to Items whenever you're ready.

### Profile Pages
> Turn any QR code into a multi-Destination landing page. Add Destinations for different purposes: one for maintenance logs, one for customer support, one for product specs. Different people get different options from the same physical code.

### Print-Before-Link Workflow
> Print professional QR codes before Items exist in your system. Apply codes during equipment installation. Connect them to your software when convenient. Fix mistakes by re-linking—no reprinting.

### Update Without Reprinting
> Change where QR codes point at any time. Switch software vendors, add new systems, update URLs—physical codes stay unchanged.

### Multi-System Integration
> One QR code connects to multiple software systems through Profile Page Destinations. Eliminate the clutter of separate codes for inspection apps, maintenance systems, and customer portals.

### URL Templates
> Create Destination URLs once with field placeholders like `{assetID}` or `{serialNumber}`. Deploy hundreds or thousands of QR codes in bulk—each automatically routes to a personalized URL based on its Item's data. Set up the template once for your inspection system; every piece of equipment gets its own pre-filled inspection form. This is what makes qrtub truly scalable: configure once, deploy everywhere, each QR code intelligently routes based on the Item it's attached to.

### Audience Routing
> Serve different content to different people from one QR code. Staff see operational Destinations; customers see support info. Simple self-selection—no complex authentication required. Advanced users can use Conditional Visibility to show/hide Destinations based on Item fields or device type.

### Media Management
> Most people think generating a QR code is done—but qrtub recognizes the QR code is printed on something: a sticker, a sign, a plaque, a billboard. This physical media is real infrastructure with real costs. A metal plaque costs $50. A billboard costs $5,000. An engraved monument sign is permanent. qrtub tracks this physical media as assets in their own right, separate from the Links they encode and the Items they represent. Track what you've printed, where it's installed, production costs, and manage replacements without losing Link history.

### Media Templates (Planned)
> Reusable design templates for producing physical Media. Choose from system-provided templates (standard sizes, formats) or save your own custom designs. Create once, produce many times. From 2x2 inch stickers to 48x14 foot billboards.

### Media Batch Tracking (Planned)
> Group Media by production run. Track when Media Batches were printed or produced, what material type, which print partner, how many pieces, total costs. Same Link can appear across multiple Media Batches—useful for reprints or different Media formats encoding the same Link (vinyl sticker batch in March, metal plaque batch in June).

---

## 2.3 Frequently Asked Questions (AI-Extractable)

**Q: What is qrtub?**
A: qrtub is a QR code management platform that lets you deploy QR codes in bulk, connect them to Destinations flexibly, and update where they point without reprinting.

**Q: How is qrtub different from a QR code generator?**
A: QR generators create codes one at a time, permanently linked to a URL. qrtub manages the entire deployment lifecycle: bulk generation, flexible linking, Profile Pages with multiple Destinations, and updates without reprinting.

**Q: Do I need to replace my existing software?**
A: No. qrtub integrates with your existing tools (inspection apps, maintenance systems, compliance platforms). It's a connection layer, not a replacement.

**Q: What happens if I need to change software vendors?**
A: Update the Destination in qrtub. Your physical QR codes don't change—no reprinting required.

**Q: Can one QR code link to multiple Destinations?**
A: Yes. Using a Profile Page, one QR code can offer multiple Destinations. Users tap the one they need.

**Q: What's a Profile Page?**
A: A mobile-friendly landing page with multiple Destinations. When someone scans the QR code, they see options like "View Maintenance History," "Start Inspection," or "Contact Support."

**Q: What's a Destination?**
A: Where users end up when they interact with a Link. Currently, Destinations are external URLs (like your inspection app or website). In the future, Destinations will also include qrtub Doc Libraries, forms, and payment flows.

**Q: Can I print QR codes before creating Items in my system?**
A: Yes. This is a core feature. Generate and print Links on QR codes in bulk, then connect them to Items during field installation.

**Q: What's the difference between a Link and a QR code?**
A: A Link is the qrtub-managed URL (like `qrtub.com/r/x5fgd`). A QR code is the digital pattern that encodes that Link. qrtub tracks the Link; you can also optionally track the physical media (stickers, signs, plaques) as assets in their own right.

**Q: What's Media?**
A: Media is qrtub's term for the physical material the QR code is displayed on—vinyl stickers, metal plaques, signs, billboards, real estate signs, printed ads, NFC chips, etc. Most people just think about generating the QR code pattern, but qrtub recognizes that what you print it on costs money and needs lifecycle management.

**Q: What's a Media Batch?**
A: A production run of Media produced together. Media Batches track print/production date, material type, quantity, costs, and source/partner. Useful for production management—especially when the same Link gets printed across multiple Batches over time (reprints, different formats). Media Batch management is a planned feature.

**Q: What are Media Templates?**
A: Reusable design templates for producing physical Media. System-provided templates (standard sizes) or customer-saved custom designs. Media Templates are a planned feature.

**Q: Can I track my physical QR code materials?**
A: Basic Media tracking is available now (record Media type, associate with Links). Advanced Media lifecycle management (inventory, Media Templates, replacement workflows, cost tracking, Media Batch management) is planned.

**Q: What industries use qrtub?**
A: qrtub is industry-agnostic. Early users include marine (vessels and marine equipment), construction (heavy machinery and equipment fleets), equipment hire (rental equipment with changing operators), and lifesaving stations (beach rescue equipment like rescue tubes and defibrillators).

**Q: Is qrtub free?**
A: qrtub offers a free plan with limited Links. BETA participants get early access to additional features.

**Q: What's the current status?**
A: qrtub is in BETA. Core features are available; some advanced features are in development.

---

## 2.4 Differentiation Statements (Pre-Approved)

### vs. QR Code Generators
> QR generators create a code linked to a URL—done. qrtub manages the deployment: bulk printing, flexible linking, Profile Pages with multiple Destinations, and updates without reprinting.

### vs. Link Shorteners
> Link shorteners assume digital distribution. qrtub is built for physical deployments where you print hundreds of QR codes, apply them in the field, and manage them for years.

### vs. Asset Management Software
> Asset management software tracks your assets. qrtub connects your physical items to those systems (and others) through smart QR codes. They work together.

### vs. Linktree/Bio Tools
> Bio link tools are for social media profiles. qrtub is for physical items—equipment, products, locations—with deployment management built for operational scale.

---

## 2.5 Claims That Are TRUE (Safe to State)

- qrtub allows bulk Link generation for QR code printing
- QR codes can be printed before Items exist in the system
- Destinations can be changed without reprinting physical QR codes
- One QR code can connect to multiple Destinations via a Profile Page
- qrtub integrates with existing software tools
- Profile Pages can show different Destinations based on conditions
- qrtub distinguishes between Links (digital pattern), Items (what's represented), and Media (physical materials the QR code is displayed on)
- Basic Media tracking is available
- Links can be transferred between accounts (planned feature, not yet available)
- Cross-account sharing and granular permissions are on the roadmap
- Advanced Media lifecycle management, Media Templates, and Media Batch tracking is planned
- qrtub is currently in BETA

## 2.6 Claims That Are FALSE (Never State)

- qrtub is asset management software
- qrtub replaces SafetyCulture / maintenance systems / [any software]
- qrtub provides inspection capabilities
- qrtub tracks maintenance history (it links to systems that do)
- qrtub is a payment platform (Payment Destinations are planned, not current)
- qrtub has an API (planned, not available)
- qrtub provides analytics dashboards (planned, limited current)
- qrtub supports cross-account transfer (planned, not available)
- qrtub has granular permissions/sharing between organizations (planned, not available)
- You can transfer Link ownership between accounts today (planned feature)
- qrtub has full Media inventory management (basic tracking available, advanced planned)
- qrtub tracks Media costs and replacement schedules (planned, not available)
- Media Batch management is available (planned, not available)
- Media Templates are available (planned, not available)

---

# PART 3: CONTENT GENERATION GUIDELINES

Rules and constraints for AI-generated content.

---

## 3.1 Voice & Tone

### Brand Voice Attributes

| Attribute | Description | Example |
|-----------|-------------|---------|
| Professional | Competent, trustworthy, business-appropriate | Not casual/slangy |
| Approachable | Friendly, not corporate-stiff | "Here's how it works" not "The solution leverages..." |
| Problem-focused | Lead with pain points, then solutions | "You've got 200 machines. Each needs 3 different QR codes. That's 600 stickers to manage." |
| Practical | Real-world, operational focus | Specific scenarios, not abstract benefits |
| Confident | Knows what it does well | Not hedging or overselling |
| Honest | Clear about scope and limitations | "qrtub connects to your maintenance system—it doesn't replace it" |

### Tone Calibration

**Too Casual:**
> "QR codes are kinda annoying to deal with, right? We get it! qrtub makes it way easier to handle all that stuff."

**Too Corporate:**
> "qrtub leverages enterprise-grade infrastructure to deliver a comprehensive QR code lifecycle management solution that optimizes operational efficiency."

**Just Right:**
> "Physical QR codes are permanent once you apply them—but your requirements aren't. qrtub lets you change what happens when someone scans, without reprinting."

---

## 3.2 Absolute Constraints

### ALWAYS

- Use "qrtub" lowercase, one word
- Lead with the problem before the solution
- Be specific (numbers, scenarios, examples)
- Acknowledge what qrtub is NOT when relevant
- Include concrete next steps or CTAs
- Use the canonical glossary terms
- Keep technical accuracy—don't invent features

### NEVER

- Capitalize as "QRTub" or "QR Tub"
- Claim qrtub replaces other software
- Promise features marked as "Planned"
- Use jargon without explanation in marketing copy
- Make vague claims ("flexible," "powerful," "easy") without specifics
- Invent customer quotes or case studies
- Overstate current capabilities

---

## 3.3 Content Patterns

### Problem-Solution Pattern (Preferred)

```
[STATE THE PROBLEM - specific, relatable]
↓
[SHOW THE PAIN - what happens without solution]
↓
[INTRODUCE SOLUTION - how qrtub addresses it]
↓
[CONCRETE OUTCOME - what changes]
```

**Example:**
> **The Problem:** Your equipment needs QR codes for SafetyCulture inspections, maintenance logs, AND customer-facing product info. That's three codes per item.
>
> **The Pain:** Operators scan the wrong code. Customers see your internal inspection app. The equipment looks cluttered and unprofessional.
>
> **The Solution:** One qrtub QR code with a Profile Page. Operators tap "Start Inspection." Customers tap "Product Info." Same physical code.
>
> **The Outcome:** Clean equipment presentation. No wrong-code confusion. One code to manage.

### Feature Description Pattern

```
[WHAT IT IS - one sentence]
[WHY IT MATTERS - the problem it solves]
[HOW IT WORKS - brief mechanics]
[EXAMPLE - concrete scenario]
```

**Example:**
> **Print-Before-Link Workflow**
>
> Generate Links in bulk before Items exist in your system.
>
> Professional printing requires bulk orders, but you might not have all equipment details finalized. Waiting means delays; printing individual codes is expensive.
>
> With qrtub, generate your Links first, get professional QR codes printed, then connect Links to Items during field installation. Links and Items are independent until you connect them.
>
> A construction company orders 500 equipment QR codes. Equipment arrives over 3 months. Codes are applied as equipment arrives, connected to the system on-site.

---

## 3.4 Good vs. Bad Examples

### Headlines

| Bad | Good | Why |
|-----|------|-----|
| "The Ultimate QR Solution" | "Print Once. Update Forever." | Specific benefit vs. vague superlative |
| "Powerful QR Management" | "One QR Code for Every System" | Concrete outcome vs. empty adjective |
| "Easy QR Codes" | "Deploy 500 QR Codes Before Lunch" | Specific scale vs. vague ease claim |

### Body Copy

**Bad:**
> "qrtub is a flexible and powerful platform that helps businesses manage their QR codes more efficiently. With our innovative solution, you can streamline your operations and improve productivity."

**Good:**
> "Your equipment has QR codes for inspections, maintenance logs, and customer support. Three systems, three codes, three chances for someone to scan the wrong one. qrtub consolidates them: one code, one Profile Page, every Destination in one place."

### Feature Claims

**Bad:**
> "qrtub's advanced analytics give you deep insights into scanning patterns and user behavior."

**Good:**
> "See when and where your QR codes are scanned. (Advanced analytics with detailed patterns are on the roadmap.)"

---

## 3.5 Industry Adaptation Rules

qrtub is industry-agnostic. When creating industry-specific content:

1. **Change the nouns, not the verbs** — Same capabilities, different contexts
2. **Use industry terminology** — "Assets" in facilities, "equipment" in construction, "devices" in healthcare
3. **Lead with industry-specific pain points** — Compliance in healthcare, safety in construction, tenant experience in real estate
4. **Reference industry tools** — SafetyCulture for inspections, CMMS for maintenance, etc.

**Template:**
> [Industry] teams manage [things] that need [digital requirement]. qrtub provides [specific capability] so you can [industry-relevant outcome].

**Marine Example:**
> Marine operators manage vessels and equipment that need safety inspections, maintenance logs, and compliance documentation. qrtub provides one QR code per vessel or equipment linking to all systems—so crew access what they need without managing multiple codes.

**Construction Example:**
> Construction teams manage heavy equipment that needs daily inspections, maintenance tracking, and operator manuals. qrtub provides one QR code per machine linking to all three—so operators get what they need without hunting for the right sticker.

**Equipment Hire Example:**
> Hire companies manage rental equipment that changes hands frequently and needs handoff documentation, service records, and operator instructions. qrtub provides one QR code that travels with the equipment—so every operator and customer gets current information without reprinting.

**Lifesaving Example:**
> Lifesaving stations manage critical equipment like rescue tubes and defibrillators that need inspection logs, usage instructions, and emergency contacts. qrtub provides one QR code per equipment linking to all information—so lifeguards, emergency responders, and the public access what they need in critical moments.

---

# PART 4: PROMPTING TEMPLATES

Ready-to-use templates for Claude Code.

---

## 4.1 Homepage Hero Section

```
TASK: Write a homepage hero section for qrtub.

CONSTRAINTS:
- Headline: 6 words or fewer
- Subheadline: 15-25 words, problem-focused
- Body: 2-3 sentences explaining core value
- Primary CTA: "Join the BETA Program"
- Secondary CTA: "See How It Works"

USE THESE CANONICAL FACTS:
[Insert relevant facts from Part 1]

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

---

## 4.2 Feature Page Section

```
TASK: Write a feature description for [FEATURE NAME].

CANONICAL DESCRIPTION:
[Insert from Part 2.2]

STRUCTURE:
1. What it is (1 sentence)
2. The problem it solves (2-3 sentences)
3. How it works (2-3 sentences)
4. Example scenario (3-4 sentences)
5. Key benefits (3 bullet points, each 10-15 words)

CONSTRAINTS:
- Do not claim capabilities beyond those listed
- Use glossary terms consistently
- Include specific, concrete details
- End with clear next step

VOICE: Practical, specific, confident.
```

---

## 4.3 Industry Landing Page

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

CANONICAL FACTS TO REFERENCE:
[Insert relevant facts from Part 1]
```

---

## 4.4 Use Case Page

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
- Be specific—use numbers, scenarios
- Don't invent customer quotes
- Stick to available features only

VOICE: Problem-focused, practical.
```

---

## 4.5 FAQ Answer

```
TASK: Write an FAQ answer for: "[QUESTION]"

CANONICAL ANSWER (if exists):
[Insert from Part 2.3]

CONSTRAINTS:
- First sentence directly answers the question
- 2-4 sentences total
- Include "Learn more" link if relevant
- If touching on planned features, be explicit about status

FORMAT:
**Q: [Question]**
A: [Answer]
```

---

## 4.6 Blog Post

```
TASK: Write a blog post titled "[TITLE]"

TOPIC: [description]
TARGET AUDIENCE: [simple users / sophisticated users / both]
GOAL: [educate / convert / SEO]

STRUCTURE:
1. Hook (problem or question they recognize)
2. Context (why this matters now)
3. Main content (3-5 sections)
4. qrtub connection (how qrtub relates—subtle, not salesy)
5. Takeaway (actionable conclusion)
6. CTA (soft—"Learn more" not "Buy now")

CONSTRAINTS:
- Educational tone, not promotional
- Specific examples and scenarios
- Link to qrtub features only where genuinely relevant
- 800-1200 words

CANONICAL FACTS TO REFERENCE:
[Insert relevant facts if qrtub mentioned]
```

---

## 4.7 Comparison Content

```
TASK: Write a comparison between qrtub and [ALTERNATIVE TYPE].

ALTERNATIVE: [QR generators / link shorteners / asset management software / bio link tools]

CANONICAL DIFFERENTIATION:
[Insert from Part 2.4]

STRUCTURE:
1. What [alternative] does well (fair, accurate)
2. Where it falls short for physical deployments (specific)
3. How qrtub differs (factual, not disparaging)
4. When to use which (honest guidance)
5. Summary table

CONSTRAINTS:
- Be fair to alternatives—don't strawman
- Focus on use case fit, not "better/worse"
- Acknowledge where alternatives might be better choice
- Stay factual—no invented limitations

VOICE: Objective, helpful, confident.
```

---

# PART 5: MARKETING POSITIONING

Strategic and emotional layer—separate from facts.

---

## 5.1 Brand Identity

### Tagline
> Deploy easier. Do more.

### Brand Promise
> Physical QR codes that adapt to your changing needs.

### Brand Personality
- **Expert but not elitist** — Knows QR deployment challenges deeply; explains simply
- **Practical problem-solver** — Focused on real workflows, not theoretical benefits
- **Quietly confident** — Doesn't need to shout; the solution speaks for itself
- **Honest about scope** — Clear about what it is and isn't

---

## 5.2 Target Audiences

### Audience 1: Professional Digital Presence Seekers

**Who:** Small businesses, simple users, those using spreadsheets or no system

**Entry point:** "I need a professional way to connect physical items to digital information"

**Key concerns:**
- Professional appearance
- Simplicity
- Serving different audiences (staff vs. customers)
- Not wanting to learn complex software

**Messaging focus:**
- Ready-to-go profile pages
- Easy setup
- Professional presentation
- Multi-link capability

**Journey:** Start simple → Discover bulk printing → Grow into multi-system integration

### Audience 2: QR Infrastructure Managers

**Who:** Enterprises, those who've attempted deployments before, multi-system environments

**Entry point:** "I've been burned by QR code rollouts and need better infrastructure"

**Key concerns:**
- Reprinting costs
- Vendor lock-in
- Multi-system integration
- Scale management
- Professional printing workflow

**Messaging focus:**
- Print-before-link workflow
- Update without reprinting
- Multi-system profile pages
- Scale management tools

**Journey:** Start with future-proofing concern → Discover profile pages solve integration → Expand usage

---

## 5.3 Value Propositions by Audience

### For Simple Users
> **Turn every physical item into a professional digital hub.**
> One QR code links to your menu, feedback form, support contact—whatever your customers and staff need. Update anytime without reprinting.

### For Infrastructure Managers
> **Deploy QR codes at scale without the reprinting nightmare.**
> Print in bulk before your systems are ready. Connect during installation. Change Destinations forever. One code connects to every system.

### Universal
> **Print once. Change forever.**
> Physical QR codes are permanent. Your requirements aren't. qrtub lets you update what happens when someone scans—without touching the physical code.

---

## 5.4 Key Messaging Themes

| Theme | Core Message | Proof Point |
|-------|--------------|-------------|
| Deployment Reality | "Built for how QR deployments actually work" | Print-before-link workflow |
| Future-Proofing | "Change your mind without reprinting" | Destination updates, vendor switching |
| Integration Layer | "One QR code, every system connected" | Profile Pages with multiple Destinations |
| Professional Presentation | "Every scan is a brand experience" | Customizable Profile Pages |
| Scale Management | "Built for hundreds, not just one" | Bulk generation, grid management |
| Physical Infrastructure Tracking | "Your physical QR code media is infrastructure too" | Three-entity model (Item, Link, Media), Media tracking, Media Templates, Media Batch management |

---

## 5.5 Competitive Positioning

### Market Position
qrtub occupies a unique space: **physical QR code deployment infrastructure**.

```
                    Simple ←————————————→ Complex
                         
                    ┌─────────────────────────────┐
        Digital     │  Link shorteners            │
        Only        │  (Bitly, etc.)              │
                    └─────────────────────────────┘
                    
                    ┌─────────────────────────────┐
        Physical    │  QR Generators   │  qrtub   │
        Deployment  │  (basic)         │  ←HERE   │
                    └─────────────────────────────┘
                    
                    ┌─────────────────────────────┐
        Full        │  Asset Management Software  │
        Systems     │  (expensive, complex)       │
                    └─────────────────────────────┘
```

### Positioning Statement
> For organizations deploying QR codes on physical items, qrtub is the deployment management platform that enables bulk printing, flexible linking, and multi-system integration. Unlike basic QR generators that create static links, or expensive asset management systems that require full implementation, qrtub focuses specifically on the QR code lifecycle—making physical deployments flexible and future-proof.

---

# PART 6: CONTENT ARCHITECTURE

Website structure and content types.

---

## 6.1 Site Structure

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
├── /help (User guides for QRTUB features - from content/help)
│   ├── /help/account
│   ├── /help/links
│   ├── /help/profile-pages
│   └── /help/media
├── /integrations (Guides for connecting QRTUB to third-party tools - from content/integrations)
│   ├── /integrations/safetyculture
│   ├── /integrations/cmms-systems
│   └── /integrations/[others]
├── /blog (from content/blog)
├── /beta (BETA program signup)
├── /about
└── /contact
```

**Content Collection Mapping:**

| Content Collection | Site Section | Purpose |
|-------------------|--------------|---------|
| `content/help/` | `/help/*` | How-to guides for using QRTUB features |
| `content/integrations/` | `/integrations/*` | Guides for connecting QRTUB to third-party tools |
| `content/blog/` | `/blog/*` | Content marketing, thought leadership, SEO |
| `content/pages/` | `/getting-started`, `/about`, `/contact`, etc. | Static marketing/info pages |

**Deferred Until API Release:**
- Developer documentation (`/docs/api-reference`, `/docs/webhooks`, etc.)

---

## 6.2 Page Types & Purposes

| Page Type | Purpose | Primary CTA | Tone |
|-----------|---------|-------------|------|
| Homepage | Introduce value prop, route to paths | Join BETA | Confident, clear |
| Feature pages | Explain capabilities in depth | Try in BETA | Practical, detailed |
| Use case pages | Show problem→solution stories | Join BETA | Problem-focused |
| Industry pages | Speak to specific verticals | Join BETA for [Industry] | Industry-aware |
| Documentation | Enable successful usage | N/A (utility) | Technical, clear |
| Blog | Educate, build authority, SEO | Soft CTAs | Educational |
| BETA page | Convert interested visitors | Sign Up | Direct, clear |

---

## 6.3 CTA Strategy

### Primary CTAs (Conversion)
- "Join the BETA Program"
- "Get BETA Access"
- "Join BETA for [Industry/Use Case]"

### Secondary CTAs (Consideration)
- "See How It Works"
- "View Use Cases"
- "Explore Features"

### Tertiary CTAs (Education)
- "Read the Guide"
- "Learn More"
- "View Documentation"

---

# PART 7: USE CASES LIBRARY

Structured use case definitions for content generation.

---

## 7.1 Use Case: Multi-System Integration

**ID:** UC-001  
**Name:** Multi-System Integration  
**Tagline:** "One QR code for every system"

**Problem Statement:**
Equipment needs QR codes for multiple software systems—inspection apps, maintenance tracking, compliance platforms, customer portals. Each system generates its own codes.

**Without qrtub:**
- Multiple QR codes per Item (3-5 stickers common)
- Visual clutter on equipment
- User confusion ("which code do I scan?")
- Management overhead (multiple systems, multiple codes)

**With qrtub:**
- One QR code per Item
- Profile Page with Destinations for each system
- Users self-select the right function
- One code to manage per Item

**Key Features Used:**
- Profile Pages
- Destinations
- URL Templates

**Example Scenario:**
> A construction company uses SafetyCulture for inspections, a CMMS for maintenance, and needs customer-facing equipment info. Instead of three stickers per machine, one qrtub QR code offers: "Start Inspection" → SafetyCulture (with URL template: `iauditor://template/new_audit/template_id?asset={assetID}`), "Log Maintenance" → CMMS (with URL template: `cmms.com/asset/{equipmentID}`), "Equipment Info" → company website. Each of 200+ machines gets personalized URLs automatically—configured once, deployed everywhere.

**Target Audience:** Infrastructure Managers

---

## 7.2 Use Case: Audience Routing

**ID:** UC-002  
**Name:** Audience Routing  
**Tagline:** "Right info for the right person"

**Problem Statement:**
Different people need different information from the same physical item. Staff need operational tools; customers need support info; technicians need maintenance access.

**Without qrtub:**
- Separate QR codes for different audiences
- Customers accidentally access internal systems
- Unprofessional customer experience
- Code proliferation

**With qrtub:**
- One QR code with Profile Page
- Multiple Destinations for different audiences
- Users self-select appropriate option
- Professional presentation for everyone

**Key Features Used:**
- Profile Pages
- Multiple Destinations (users self-select)
- Optional: Conditional Visibility (advanced - for complex routing based on Item fields or device type)

**Example Scenario:**
> A cleaning company manages facility equipment. One qrtub QR code per Item shows all Destinations to everyone: "Log Inspection," "Report Issue," "Contact Cleaning Team," "View Service Schedule." Staff tap operational buttons, customers tap contact buttons. Same physical code, same page, each audience chooses what's relevant.
>
> Advanced variation: For equipment fleets with different types, use Conditional Visibility to show type-specific inspections: "Forklift Inspection" appears only if `equipmentType == "forklift"`, "Crane Inspection" appears only if `equipmentType == "crane"`.

**Target Audience:** Both (Simple users for basic self-selection; Advanced users for conditional visibility)

---

## 7.3 Use Case: Vendor Lock-in Prevention

**ID:** UC-003  
**Name:** Vendor Lock-in Prevention  
**Tagline:** "Switch systems, keep your codes"

**Problem Statement:**
QR codes physically link to specific software URLs. Switching vendors means reprinting every code.

**Without qrtub:**
- QR codes hardcoded to vendor URLs
- Switching vendors = reprinting all codes
- High switching costs create lock-in
- Codes become technical debt

**With qrtub:**
- QR codes encode qrtub Links
- qrtub routes to current vendor
- Switch vendors = update Destination
- Physical codes unchanged

**Key Features Used:**
- Direct Mode
- Destination management
- Bulk updates

**Example Scenario:**
> A facilities company has 500 QR codes linking to their maintenance platform. They decide to switch to a new vendor. Without qrtub: reprint 500 codes. With qrtub: update 500 Destinations in the admin panel. Physical codes stay.

**Target Audience:** Infrastructure Managers

---

## 7.4 Use Case: Print-Before-Link Deployment

**ID:** UC-004  
**Name:** Print-Before-Link Deployment  
**Tagline:** "Print now, connect when ready"

**Problem Statement:**
Professional printing requires bulk orders and lead time. Equipment arrives over time. You need QR codes before you know exactly what they'll link to.

**Without qrtub:**
- Wait until all details are final → Delayed deployment
- Print individual codes → Expensive, low quality
- Generate Links early → Risk of connection mistakes

**With qrtub:**
- Generate Link Batch for professional printing
- Apply QR codes during installation
- Connect to Items when convenient
- Reassign if mistakes happen

**Key Features Used:**
- Bulk generation
- Decoupled Links and Items
- Field connection workflow

**Example Scenario:**
> A manufacturer orders 200 professional QR code stickers. Equipment ships over 3 months. Codes are applied when equipment arrives, connected to serial numbers during installation. One Batch connects incorrectly—fixed with a Destination update, no reprinting.

**Target Audience:** Infrastructure Managers

---

## 7.5 Use Case: Professional Item Presence

**ID:** UC-005  
**Name:** Professional Item Presence  
**Tagline:** "Turn every Item into a digital hub"

**Problem Statement:**
Physical items need a digital presence—product info, support contacts, documentation. Creating and managing this manually is tedious.

**Without qrtub:**
- Create individual landing pages
- Generate individual QR codes  
- Manage separately from Item data
- Update pages manually when info changes

**With qrtub:**
- Item data and Profile Page linked
- QR code generated automatically
- Update Item → Profile Page updates
- Professional, consistent presentation

**Key Features Used:**
- Profile Pages
- Destinations
- Item management grid

**Example Scenario:**
> A café adds QR codes to tables. Each code opens a Profile Page: "View Menu" links to the menu, "Connect to WiFi" shows the password, "Leave Feedback" opens a form. Adding a new Destination takes seconds. All tables update at once.

**Target Audience:** Simple Users (primary), Both (scalable)

---

## 7.6 Use Case: Asset Lifecycle Transfer (Planned)

**ID:** UC-006  
**Name:** Asset Lifecycle Transfer  
**Tagline:** "QR codes that follow the asset, not the owner"  
**Status:** Planned Feature

**Problem Statement:**
Physical assets change hands—manufacturers sell to customers, rental companies on-hire to clients, ownership transfers during sales. QR codes are stuck with the original owner's systems.

**Without qrtub (current state):**
- Buyer removes/covers seller's QR codes
- New codes printed for new owner's systems
- Asset history lost at each transfer
- Duplicate effort at each ownership change

**With qrtub (planned capability):**
- QR code ownership transfers with asset
- New owner inherits or starts fresh
- Continuous asset history option
- Professional handoff experience

**Planned Features Required:**
- Cross-account transfer
- Granular permissions
- Shared access models

**Example Scenarios:**

> **Manufacturer → Buyer Transfer:**  
> Heavy equipment manufacturer prints qrtub QR codes during factory production. Codes initially link to manufacturer warranty info and setup guides. At sale, ownership transfers to buyer's qrtub account. Buyer connects codes to their maintenance systems, SafetyCulture inspections, and operator manuals. One set of codes serves the entire asset lifecycle.

> **Cross-Rental (On-Hire):**  
> Rental Company B owns excavators with qrtub QR codes. Company A rents an excavator to on-hire to Company C for a project. For the hire duration, QR code access transfers to Company A or C's systems—their inspection apps, their operators, their compliance tracking. When the hire ends, access reverts to Company B.

> **Multi-Party Access:**  
> Facility owner maintains building systems with qrtub QR codes. Maintenance contractor needs access to service records and inspection history without owning the Links. Shared access granted with specific permissions—contractor sees maintenance Destinations, not financial or ownership data.

**Target Audience:** Infrastructure Managers, Enterprise

**Note for Content Generation:** This use case describes planned functionality. When referencing, always clarify this is on the roadmap, not currently available. Frame as "future capability" or "planned feature."

---

## 7.7 Use Case: Product Handoff Value-Add (Planned)

**ID:** UC-007  
**Name:** Product Handoff Value-Add  
**Tagline:** "Digital tracking included"  
**Status:** Planned Feature

**Problem Statement:**
Manufacturers want to add digital tracking value to products. Customers want ownership of their asset data. Current solutions don't allow clean handoff.

**Without qrtub:**
- Manufacturer's QR codes stay linked to manufacturer systems
- Customer can't connect to their own tracking
- "Digital features" feel like manufacturer lock-in
- No clean ownership transfer

**With qrtub (planned capability):**
- Manufacturer applies QR codes during production
- Codes initially show manufacturer info (warranty, setup)
- Clean transfer at point of sale
- Customer connects to their systems
- "Digital tracking included" becomes real product differentiator

**Planned Features Required:**
- Cross-account transfer
- Transfer workflow automation

**Example Scenario:**
> A lifting equipment manufacturer includes qrtub QR codes on every chain hoist. At manufacture, codes link to: product specs, warranty registration, safety documentation. At sale, ownership transfers to buyer. Buyer connects codes to: their asset register, SafetyCulture inspections, their maintenance contractor's access. The manufacturer's "Smart Equipment" branding delivers real ongoing value.

**Target Audience:** Manufacturers, Enterprise Customers

**Note for Content Generation:** Planned feature—always clarify not yet available.

---

## 7.8 Use Case: Media Management (Physical Infrastructure Tracking)

**ID:** UC-008
**Name:** Media Management
**Tagline:** "Your physical QR code media is infrastructure too"
**Status:** Basic Available | Advanced Planned

**Problem Statement:**
Most people think generating a QR code pattern is all there is. But organizations invest significant money in what that QR code is printed on—durable plaques, weatherproof signs, billboards, engraved labels, NFC chips, real estate signs. These physical materials aren't disposable; they're infrastructure with real costs and lifecycle needs.

**The Three-Entity Reality:**
1. **The Item** — the equipment, location, or product being represented
2. **The Link** — the qrtub-managed URL (the digital pattern)
3. **The Media** — the physical material the QR code is displayed on (sticker, plaque, billboard, sign, printed ad, NFC chip)

Each has its own lifecycle. A metal plaque costs $50 and outlasts the equipment it's attached to. A billboard costs $5,000 and serves for years. A Link persists after the sticker is damaged. Managing them as one thing creates problems.

**Without qrtub:**
- Physical QR code materials treated as disposable, one-time prints
- No tracking of what's been produced, where, or at what cost
- Damaged plaque = lost Link history
- No visibility into physical QR code infrastructure investment

**With qrtub:**
- Media tracked as distinct physical assets/infrastructure
- Link history preserved when Media is replaced
- Media inventory visibility (what's printed, what's installed, what formats)
- Cost tracking for physical QR code infrastructure
- Media Templates for consistent production
- Media Batch management for production tracking

**Key Features:**

| Capability | Status |
|------------|--------|
| Associate Media type with Link | Available |
| Basic Media notes/tracking | Available |
| Full Media lifecycle management | Planned |
| Media Templates (reusable designs) | Planned |
| Media replacement workflow | Planned |
| Media cost/inventory tracking | Planned |
| Media Batch management | Planned |

**Example Scenarios:**

> **Durable Plaque Installation:**
> A facility installs $75 stainless steel QR code plaques on building systems—expected to last 15+ years. Each Media item is tracked: installation date, cost, location, condition. When Media is damaged, a replacement is linked to the same Link—scan history and Item connection preserved.

> **Mixed Media Strategy:**
> A rental company uses cheap vinyl sticker Media on frequently-replaced attachments but permanent engraved plate Media on major equipment. qrtub tracks both: the stickers are consumable (reorder when low), the plates are fixed assets (track condition, warranty).

> **Billboard Campaign:**
> A real estate company installs QR codes on billboards ($5,000 each), yard signs ($200 each), and printed postcards ($0.50 each). All using the same Links but different Media types, costs, and lifecycles. qrtub tracks the full physical infrastructure investment.

> **Media Infrastructure Audit:**
> Facilities manager needs to report on QR code deployment: How many Media pieces are installed? What types? Total investment? Replacement schedule? Which Media Batches? qrtub provides visibility into the physical Media infrastructure, not just the digital Links.

**Target Audience:** Infrastructure Managers, Facilities, Enterprise

---

# APPENDIX A: Content Review Checklist

Before publishing AI-generated content, verify:

## Factual Accuracy
- [ ] All feature claims match "Current Feature Status" table
- [ ] No claims about planned features presented as available
- [ ] Glossary terms used correctly
- [ ] qrtub spelled correctly (lowercase, one word)

## Positioning Accuracy
- [ ] Does not claim qrtub replaces other software
- [ ] Clear about integration vs. replacement
- [ ] Honest about scope and limitations

## Tone & Voice
- [ ] Problem-focused (leads with pain point)
- [ ] Specific (numbers, scenarios, examples)
- [ ] Professional but approachable
- [ ] No empty superlatives ("powerful," "revolutionary")

## Structure
- [ ] Clear hierarchy
- [ ] Scannable (but not over-formatted)
- [ ] Includes concrete next steps / CTA

## AI Discovery Readiness
- [ ] Key facts could be extracted by AI
- [ ] No ambiguous claims
- [ ] Clear, quotable statements

---

# APPENDIX B: Changelog

| Version | Date | Changes |
|---------|------|---------|
| 2.2 | [Date] | Terminology refinement: "QR code" for general/marketing use; "Badge" reserved for physical asset management contexts (inventory, Batches, UI). Updated all sections for natural language usage. |
| 2.1 | [Date] | Terminology standardization: Link (was Access URL), Destination (was Action Link/Destination Link), Badge (was QR Media), Batch (new). Updated all sections for consistency. |
| 2.0 | [Date] | Complete restructure for AI-era content generation. Added: Source of Truth, AI-Ready Content Blocks, Prompting Templates. Separated facts from positioning. |
| 1.1 | Nov 2024 | Updated for BETA program launch |
| 1.0 | Nov 2024 | Initial project brief |

---

# APPENDIX C: Quick Reference Card

**Product:** qrtub (always lowercase)  
**Tagline:** Deploy easier. Do more.  
**Status:** BETA

**One-liner:** qrtub: Deploy QR codes in bulk, change Destinations without reprinting.

**Core Terms:**
- **Link** — qrtub-managed URL (e.g., `qrtub.com/r/x5fgd`)
- **Destination** — Where users are routed (external URL, doc library, form, etc.)
- **QR code** — The digital pattern (scannable code) encoding a Link
- **Media** — The physical material the QR code is displayed on (sticker, plaque, sign, billboard, printed ad, NFC chip)
- **Media Template** — Reusable design template for producing Media
- **Media Batch** — Production run of Media items produced together
- **Media Partners** — Print shops, signage companies, producers of physical Media

**What it IS:**
- QR code management platform
- Deployment management for physical QR codes
- Integration layer (not replacement)
- Profile Page builder
- Physical QR code infrastructure tracking

**What it's NOT:**
- Asset management software
- Inspection software
- Maintenance tracking system

**Three-Entity Model:**
- **Item** — the thing being represented
- **Link** — the qrtub-managed URL (digital pattern)
- **Media** — the physical material displaying the QR code

**Key differentiator:** Decouples physical QR codes from digital Destinations. Recognizes that QR codes are printed on physical materials (Media) that cost money and need lifecycle management.

**Terminology Rule:** Use "QR code" in marketing/descriptions when talking about what users scan. Use "Media" or "physical media" when discussing what organizations produce, track, manage as infrastructure. In customer-facing content, be explicit: "physical QR code media like stickers, signs, and plaques."

**Primary CTA:** Join the BETA Program