# CLAUDE.md

This file provides guidance to Claude Code when working with this repository.

## Repository Overview

**Exploring CNC** teaches CNC machining and CAD/CAM to hobbyists through a progressive learning journey — from understanding what a coordinate system is to running a repeatable production batch. It follows the same editorial standards and progressive structure as the other exploring_* sites (see `~/notes/STYLING.md` for the shared visual brand).

**Target Audience:** Hobbyists starting out with a desktop CNC router — a new build, a used machine, or a kit — who want to understand the workflow, not just copy setup videos. This is a hobby site (like `exploring_electronics`), not a professional/enterprise site: no prior machining, CAD, or software background assumed for a beginner-tagged article.

**Reference machine:** The author's own rig is a used X-Carve 2 (GRBL controller) running on Fedora Linux, with FreeCAD for CAD/CAM and Universal Gcode Sender (UGS) for machine control — an entirely open-source stack. Use this as the concrete example hardware/software for photos, screenshots, and worked examples, but keep concepts platform-agnostic where they generalize (GRBL is one of several controllers; FreeCAD is one of several CAM tools) — the same pattern `exploring_electronics` uses with Arduino as the reference board.

**Teaching Philosophy:** Every article starts from what the reader can see and touch — the machine, the bit, the material — before introducing the abstraction underneath (coordinate systems, G-code, feeds and speeds math). Build confidence with real, low-stakes cuts before introducing anything that can ruin a workpiece or a bit.

## Content Architecture: Topics

This is a hobby site, not a professional/enterprise or funnel site — there are no tiers, no personas to target, and no paywall. The site is organized purely by **topic**: the subject of the article (coordinate systems, G-code, CAD/CAM, etc.). Depth within a topic progresses naturally as articles accumulate, but that progression is signaled with a lightweight per-article difficulty tag, not a tier structure.

**The six stable topics** (each grows over time as articles are added):

1. **Machine Foundations** — axes and coordinate systems, homing, work zero, spindle/router basics, machine safety
2. **Reading G-code** — G-code commands, modal groups, coordinate systems (G54–G59), reading a post-processed file
3. **CAD/CAM Workflow** — FreeCAD sketching and modeling, the Path workbench, toolpath operations and strategies
4. **Machine Control** — Universal Gcode Sender, connecting and jogging, probing, running and pausing a job
5. **Materials & Cutting** — bit selection, feeds and speeds, chip load, materials (wood, plastics, aluminum)
6. **Maintenance & Calibration** — squaring the machine, belt tension, backlash, spindle/router care, dust collection

**Navigation rules:**

- Nav is **topic-first**: each topic is its own top-level nav group (`Machine Foundations`, `Reading G-code`, …). No tier wrapper above them.
- Add a topic to the nav **only once it has a published article** — never show empty groups.
- **Directory stays flat for now** (`docs/*.md`). Defer splitting into topic subdirectories until a topic has ~3+ articles — grouping today is nav-label only.
- If a cross-cutting reference shelf emerges (tool care, software installation guides that don't belong to one topic), give it its own top-level **Practical Tools** section — mirrors `exploring_electronics`.

**Difficulty tags:** Each article carries a small `!!! abstract "Beginner"` / `"Intermediate"` / `"Advanced"` admonition right under the H1, with one line of framing (what prior knowledge, if any, is assumed). This replaces tier badges — it's a per-article depth signal, not a site section or funnel stage.

**Development phases:** No fixed phase order — build out topics as interest and machine time allow. Start each topic with a Beginner article before going deeper into it.

## Important Preferences

**Git Operations**: The user handles all git operations (commits, pushes, etc.) themselves. Do not commit or push changes.

**MkDocs Operations**: The user handles running `mkdocs serve` and `mkdocs build` themselves. Do not run these commands.

## Audience and Difficulty Tags

**IMPORTANT**: This site is NOT software-developer-specific and has no funnel/persona machinery. It targets any serious adult beginner — no software background assumed, no software analogies required (unlike the developer-focused sites, CNC's audience is the maker/hobbyist community, not platform engineers). There is one audience, read at whatever depth the article calls for.

**Who reads this site:**
- Anyone with (or about to get) a desktop CNC router: new build, used machine, or kit
- May have general DIY/workshop experience but nothing CNC-specific
- Some readers stick to simple wood cuts from downloaded models; others go on to design their own parts in FreeCAD, chase tight tolerances, or cut aluminum in small batches — the site should serve all of them as they grow, without gatekeeping any of it behind a tier

Every article carries a **difficulty tag** — `Beginner`, `Intermediate`, or `Advanced` — as a one-line admonition under the H1 (see `content.code.annotate`-style example in an existing article). Pick the tag by what the article assumes the reader already knows, not by some predetermined publishing phase:

- **Beginner** — no prior CAD, CAM, or machining knowledge assumed. Mentor-to-learner voice, warm but serious. Safety-first: explain consequences before risky steps. Reassuring — mistakes are recoverable (stock and bits are cheap compared to a finger). Concrete before abstract: physical setup first, then the abstraction underneath it.
- **Intermediate** — assumes basic machine operation (homing, jogging, loading a bit, running a job), reading simple G-code, and knowing what work zero is. Peer-to-peer, no hand-holding. Safety warnings still apply, framed professionally rather than reassuringly. Prefer manufacturer datasheets and real feeds/speeds numbers over simplified rules of thumb.
- **Advanced** — assumes the full CAD/CAM workflow, feeds/speeds theory, and tool selection are second nature. Colleague-to-colleague, production context (tool life, cycle time, consistency across a batch), full technical depth, no apologies for complexity.

**Safety is never optional at any tag** — only the framing (reassuring vs. professional vs. process-control) changes.

---

## SEO Strategy and Publication Process

**CRITICAL**: This site uses a draft/publish workflow to ensure only vetted content appears in search engines and the sitemap.

### SEO Configuration Overview

1. **Sitemap**: Auto-generated by MkDocs at `/sitemap.xml` when `site_url` is configured
2. **robots.txt**: Located at `docs/robots.txt`, points to sitemap
3. **Meta plugin**: Injects canonical URLs to prevent duplicate content
4. **Social cards**: Open Graph images auto-generated for social media sharing
5. **Google Analytics**: Shared property across all exploring_* sites
6. **Exclude plugin**: Prevents unpublished content from appearing in builds and sitemap

### Required Metadata for Every Article

**MANDATORY**: Every article MUST have frontmatter metadata before being published:

```yaml
---
date: "2026-01-15 12:00"
title: "Title With a Colon: Must Be Quoted"
description: Compelling description for search results (150-160 chars ideal)
---
```

**CRITICAL**: If the title contains a colon (`:`) it **must** be quoted — unquoted colons cause PyYAML to misparse the frontmatter silently.

**Rules:**

- **Date**: Required — the RSS feed dates entries from this field. Omitting it logs an RSS build warning and leaves the feed entry undated.
- **Title**: Should be unique across the site, descriptive, include key terms
- **Description**: Summarize what the reader will learn, compelling call-to-action
- **Check length**: Titles >60 chars and descriptions >160 chars get truncated in search results

### The Exclude Plugin Strategy

**Problem**: MkDocs by default includes ALL `.md` files in builds and sitemaps, even draft/unpublished content.

**Solution**: The `mkdocs-exclude` plugin (currently commented out in `mkdocs.yaml` — this site has no draft articles yet) excludes unpublished directories/files from builds, sitemap generation, search indexing, and navigation.

**When the first draft article is created**, uncomment the `exclude` plugin block in `mkdocs.yaml` and list the draft path/glob there. Keep the exclude list and the nav's commented-out entries in sync.

### How to Publish an Article

When an article is ready for publication (passes all quality checks), follow these steps **in order**:

1. **Pre-Publication Checklist** — complete the [Quality Standards Checklist](#quality-standards-checklist) below. Do NOT proceed until all items are checked.
2. **Remove from Exclude List** — edit `mkdocs.yaml`, remove the article's line from the `exclude` plugin's `glob` list entirely (don't just comment it).
3. **Add to Navigation** — uncomment the article in the `nav:` section of `mkdocs.yaml`.
4. **Verify Publication**:
   ```bash
   poetry run mkdocs build --strict
   grep -o '<loc>[^<]*</loc>' site/sitemap.xml | grep the-article-slug
   ```
5. **Update this file's exclude-configuration notes** to reflect what's NOW excluded.

### Common SEO Mistakes to Avoid

1. Linking to unpublished articles — always check if the target is excluded before linking
2. Forgetting to update the exclude list when publishing
3. Missing metadata — every article needs date, title, and description
4. Leaving articles in navigation but still excluded — navigation and exclude list must align

---

## CRITICAL: No Repetition - Respect Reader's Time

**This is an absolute deal-breaker for content quality.**

1. **Cross-link instead of repeating** — if a concept is explained elsewhere, link to it
2. **Only repeat for significantly different perspectives** — brief intro vs. deep dive is acceptable; same explanation twice is not
3. **Progressive depth, not repetition** — each article builds WITHOUT re-explaining previous articles
4. **Audit before publishing** — use the Explore agent to search for repeated concepts across published articles in the same section before marking any article complete

---

## Project Structure

- `docs/` - Markdown content, flat (grouped by topic in nav, not in directories — see Content Architecture above)
  - `images/` - Diagrams and machine/workpiece photos
  - `stylesheets/` - Custom CSS (`extra.css`)
- `mkdocs.yaml` - Site configuration and navigation
- `pyproject.toml` - Poetry dependencies

**Important:** All articles live flat in `docs/`; topic grouping is nav-label only (see Content Architecture above). Articles reference each other using relative paths (e.g., `filename.md`).

## Common Commands

```bash
# Install dependencies
poetry install

# Serve locally (http://localhost:8000)
poetry run mkdocs serve

# Build static site (ALWAYS use --strict for link validation)
poetry run mkdocs build --strict
```

**Link Validation:** The project uses `mkdocs-htmlproofer-plugin` to validate all internal links. Always build with `--strict` flag to catch broken links.

---

## Content Guidelines

### Tone and Style

Tone varies by each article's difficulty tag. See **Audience and Difficulty Tags** above. The key rule:

- **Beginner** → mentorship voice, safety-first, concrete-before-abstract, reassuring
- **Intermediate** → peer-to-peer, no hand-holding, expects basic machine literacy
- **Advanced** → colleague-to-colleague, precision/production focus, full technical depth

**Core Principles (every article):**

- **Safety-first**: Spinning cutters, dust, and ejected material can injure. Aluminum/magnesium fines and dust in general are a fire and inhalation hazard. Never omit safety context.
- **Purpose-driven**: Always explain the "why" before the "how"
- **Practical focus**: Real feeds/speeds numbers, real bit sizes, real material thicknesses — not vague theory
- **No emoji spam**: limit to 1-3 per article, used strategically

**Beginner tone specifically:**

- Empathetic openings anchored in the physical machine: "You just homed the machine. Now what does 'work zero' actually mean?"
- Safety-first: explain what can go wrong and why, before any risky steps
- Mentorship voice: "I'll show you..." not "You must..."

**Intermediate tone specifically:**

- Peer-to-peer: assume they can home the machine, load a bit, and run a job
- Safety warnings still apply, but framed professionally — not reassuringly

**Required Sections (every article):**

1. Opening hook with real-world context (Beginner: empathetic; Intermediate/Advanced: scenario-based)
2. Core content with real numbers (dimensions, feeds, speeds, tolerances) and code/G-code snippets where relevant
3. Safety warnings where appropriate
4. Practice exercises with expandable solutions (`??? question`)
5. Quick Recap / Key Takeaways
6. What's Next — progression to next article
7. Further Reading — organized by category

---

### CNC-Specific Writing Guidelines

#### Safety Warnings — NEVER Skip These

Use the `!!! danger` admonition for injury-risk hazards:

```markdown
!!! danger "Spinning Cutters"
    Never reach toward a bit while the spindle or router is powered, even between passes.
    Always confirm the tool has fully stopped before adjusting a workpiece, changing a bit,
    or clearing chips by hand.
```

Use `!!! warning` for equipment/workpiece-destroying risks:

```markdown
!!! warning "Climb vs. Conventional Milling"
    Cutting in the wrong direction relative to bit rotation can grab the workpiece or snap
    a bit. Always confirm your CAM software's cut direction before running an unfamiliar job.
```

**Safety escalation by difficulty tag:**

- Beginner: Full explanation with WHY it's dangerous, not just "be careful"
- Intermediate: Professional callout — brief, assumes competence
- Advanced: Process-control framing (repeatability, tool life, consistent finish) alongside safety

#### Units and Values in Prose

**ALWAYS use consistent, explicit units:**

- Feed rate: mm/min or in/min (state which; the X-Carve/GRBL stack defaults to mm/min)
- Spindle speed: RPM
- Depth of cut / stepover: mm or in, always with a unit
- Dimensions: mm preferred for machine/CAD work (FreeCAD's default), with in equivalents where the reader's stock is likely imperial (US hobbyist lumber, plywood thickness)

**Formatting in prose:**

- ✅ Correct: "a 0.25 in downcut endmill", "18,000 RPM", "feed rate of 1000 mm/min"
- ❌ Wrong: "a quarter inch bit", "18000rpm", "feed of 1000"

**Command/software/G-code names in prose:**

- ✅ Correct: "Use `G54` to set your work coordinate system"
- ✅ Correct: "Open the `Path` workbench in `FreeCAD`"
- ❌ Wrong: "Use G54 to set your work coordinate system" (unbackticked)

#### Code and G-code Examples

Articles include both **G-code listings** and **CAM/scripting code** (FreeCAD Python console, post-processor tweaks). Both must be treated as first-class teaching tools.

**Code formatting:**

```markdown
``` gcode title="Rapid to Work Zero, Then Plunge" linenums="1"
G90 G54           ; (1)!
G0 X0 Y0          ; (2)!
G1 Z-5 F300       ; (3)!
```

1. Absolute positioning, work coordinate system 1
2. Rapid move to work zero in X/Y
3. Feed straight down 5 mm at 300 mm/min
```

**Code language priority:**

- **gcode** — for any G-code listing (use the `gcode` fence language for syntax highlighting where supported, falling back to plain text)
- **Python** — FreeCAD scripting (Path workbench macros, the Python console)
- **bash** — UGS CLI usage, file conversion, Fedora/Linux setup steps

#### Diagrams and Visuals

Three distinct visual types, each with a specific job — do not substitute one for another:

- **Mermaid** — logical/block diagrams: signal flow between UGS → GRBL → machine, workflow steps (design → CAM → post → run). NOT a substitute for a real toolpath or coordinate diagram.
- **Coordinate/toolpath diagrams** — schematic top-down or isometric views showing axes, work zero, and toolpath direction. Use simple labeled line art (SVG) rather than a photo when teaching an abstraction like coordinate systems.
- **Photos** — the actual machine, workpiece, bit, and setup. Beginner-tagged articles should pair a **photo first, then the diagram** (concrete setup → symbolic/abstract notation), mirroring `exploring_electronics`'s photo-then-schematic pattern.

**Mermaid color scheme for this site (slate + amber accent, matches the brand layer):**

- Standard Node (Slate 800): `fill:#2d3748,stroke:#cbd5e0,stroke-width:2px,color:#fff`
- Highlighted Node (Amber 600): `fill:#d97706,stroke:#cbd5e0,stroke-width:2px,color:#fff`
- Darker Node (Slate 900): `fill:#1a202c,stroke:#cbd5e0,stroke-width:2px,color:#fff`
- Warning/Danger (Red 600): `fill:#c53030,stroke:#cbd5e0,stroke-width:2px,color:#fff`
- Success/Output (Green 700): `fill:#2f855a,stroke:#cbd5e0,stroke-width:2px,color:#fff`

#### Manufacturer Data and Official Sources

Always link to the manufacturer's spec/feeds-and-speeds data when introducing a bit or machine component:

```markdown
The `Amana 46200-K` 1/4" upcut spiral is a common first router bit for wood and plywood.
([Spec sheet](https://www.amanatool.com/))
```

Validate all such URLs with WebFetch before publishing — tool vendors reorganize their catalogs.

#### Read-Only vs Risky Operations

Label every operation clearly for Beginner-tagged readers:

```markdown
- ✅ **Safe (Non-Destructive):** Jogging with the spindle off, reading G-code, checking work zero with the machine idle
- ⚠️ **Caution (Can Damage Bit/Workpiece):** Running an unfamiliar feed rate, skipping a dry-run/air-cut, cutting without dust collection
- 🚨 **DANGER (Can Injure):** Reaching near a spinning bit, loose clothing/hair near the spindle, disabling limit switches
```

---

### Article Layout and Visual Structure

**CRITICAL**: Articles must not be simple command/setting references. They must teach skills with context and serve multiple audiences through varied layout.

#### Visual Elements Required

1. **Mermaid Diagrams** (when workflow/architecture exists) — place at article top to show the big picture; follow the amber/slate color scheme above.
2. **Card Grids** — use `<div class="grid cards two-col" markdown>` for grouping related concepts (bit types, G-code command families). Each card should explain "Why it matters" before showing values or code. Use single-column (no `two-col`) if cards contain code blocks.
3. **Content Tabs** — use `=== "Tab Name"` for platform/tool alternatives (e.g., "UGS" vs. "CNCjs", "FreeCAD Path" vs. "another CAM tool").

#### Layout Patterns by Article Type

**Concept/Foundations Articles (coordinate systems, work zero, G-code basics):**

1. Opening — grounded in the physical machine, what the reader can see
2. Mermaid or coordinate diagram showing where this concept fits
3. Card Grid — related concepts or terms
4. Core explanation with real values
5. Safety warnings
6. Practice Exercises
7. Quick Recap
8. What's Next
9. Further Reading

**Hands-On Project Articles ("First Cut", "Design and Cut a Part"):**

1. Opening — "Here's what you'll build"
2. Materials/tools list with exact values (bit, feed rate, stock)
3. Step-by-step — design, then CAM, then run
4. Verification — how to know it came out right
5. Troubleshooting (collapsible)
6. Practice Exercises
7. What's Next
8. Further Reading

**Workflow Articles (CAD/CAM pipeline, feeds and speeds):**

1. Opening — the problem with skipping this step (a ruined cut, a broken bit)
2. Mermaid diagram — the pipeline or decision flow
3. Technical details with worked numbers
4. Practice Exercises
5. What's Next
6. Further Reading

#### Context Before Code

**NEVER start with code, G-code, or settings.** Always provide context first:

- ❌ Bad: "Set F1000 S18000 in your CAM operation"
- ✅ Good: "A feed rate that's too slow for the spindle speed burns the material instead of cutting it. For a 1/4 in upcut bit in pine, that means balancing feed rate against spindle RPM so each flute takes a clean, consistent bite..."

---

### Cross-Linking Strategy

**Internal links:** Connect concepts progressively. Each article should link forward and backward in the learning path.

**Cross-link to other exploring_* sites where relevant:**
- `exploring_electronics` — spindle/stepper motor driver electronics, limit switch wiring, GRBL as an Arduino-based controller
- `exploring_python` — scripting G-code post-processors, batch-generating parts, FreeCAD's Python console
- `exploring_linux` — running the open-source stack (UGS, FreeCAD) on Fedora/Linux day-to-day

---

## Quality Standards Checklist

Before uncommenting an article in `mkdocs.yaml`:

**✅ Content Quality:**

- [ ] **NO REPETITION AUDIT** - Searched for repeated concepts across published articles in this section (CRITICAL!)
- [ ] Opening hook with real-world relevance (Beginner: empathetic; Intermediate/Advanced: scenario-based)
- [ ] Clear learning objectives
- [ ] Values (dimensions, feeds, speeds, tolerances) are correct and practical
- [ ] Code/G-code examples tested (or clearly marked as illustrative)
- [ ] Safety considerations addressed (NEVER skip, regardless of difficulty tag)
- [ ] Practice exercises with nested solutions (`??? tip "Solution"` inside question)
- [ ] Key takeaways or quick recap
- [ ] What's Next progression
- [ ] Further Reading organized into categories (Manufacturer Specs, Official Docs, Deep Dives, Related Articles)

**✅ Technical Accuracy:**

- [ ] Values are correct and practical for the reference hardware (or clearly generalized)
- [ ] Code/G-code compiles or runs (or validated syntax)
- [ ] Safety warnings for hazardous operations
- [ ] Manufacturer/spec links verified with WebFetch
- [ ] Platform-specific differences noted (GRBL vs. other controllers, metric vs. imperial stock)

**✅ Tone and Style:**

- [ ] Correct tone for the article's difficulty tag (Beginner vs. Intermediate vs. Advanced)
- [ ] Safety-conscious throughout
- [ ] Emoji usage limited (1-3 max, strategic)
- [ ] No over-the-top marketing language

**✅ Layout and Structure:**

- [ ] Article is NOT a simple parts list or command reference — teaches skills with context
- [ ] Uses visual elements appropriately (mermaid diagrams, card grids, tabs)
- [ ] Mermaid diagram at top (if workflow/architecture exists)
- [ ] Card grids explain "Why it matters" before values/code
- [ ] Context provided BEFORE values or code (never starts with syntax)

**✅ Formatting:**

- [ ] All code blocks have `title=` attribute (and `linenums="1"`)
- [ ] Software/G-code command names in backticks in prose (e.g., `G54`, `FreeCAD`, `UGS`)
- [ ] Values use correct, explicit units (mm/min not "1000", RPM not "18000")
- [ ] **CRITICAL: Blank lines before ALL lists** (recurring MkDocs rendering issue)
- [ ] Mermaid diagrams follow slate/amber color scheme
- [ ] Internal links use relative paths
- [ ] **External/manufacturer links validated with WebFetch before publishing**

**✅ Integration and Links:**

- [ ] Pre-publication link audit completed
- [ ] **NEVER link to unpublished articles** - only link to articles uncommented in mkdocs.yaml
- [ ] Cross-links added between published articles in the same topic
- [ ] Referenced in "What's Next" from previous article
- [ ] Cross-links to other exploring_* sites where relevant

---

## Final Notes

This site teaches **CNC machining and CAD/CAM to hobbyists**, starting from the physical machine and building up to the abstractions (coordinate systems, G-code, feeds/speeds math) underneath it.

**Beginner**-tagged articles — err on the side of:
- More safety context rather than less
- More concrete, hands-on framing rather than abstract-first explanations
- Simpler cuts before complex ones
- Reassurance that mistakes are recoverable (stock and bits are cheap; fingers are not)

**Intermediate**-tagged articles — err on the side of:
- Peer-to-peer directness
- Real feeds/speeds numbers and manufacturer data over simplified rules of thumb
- Encouraging the reader to design their own parts, not just cut downloaded models

**Advanced**-tagged articles — err on the side of:
- Precision and repeatability
- Deep technical depth
- Process control over one-off success

The goal: hobbyists who can **confidently design, cut, and troubleshoot real CNC parts** — and understand the machine and the math underneath the workflow.
