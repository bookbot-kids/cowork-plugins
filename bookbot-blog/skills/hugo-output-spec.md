# Hugo Output Specification

This skill defines the Hugo-compatible output format for all Bookbot blog articles. These conventions are derived from the live site at `bookbot-www` — follow them exactly.

---

## Site Overview

- **Site URL:** https://www.bookbotkids.com/
- **Hugo config:** `hugo.toml` — Lanczos imaging, quality 85, WebP enabled
- **Hosting:** Cloudflare Pages

---

## Content Directory Structure

Blog posts live under `/content/updates/`. Posts can be at the root level or inside a subcategory folder:

```
content/updates/
├── _index.md                                         # Updates listing page
├── dyslexia/                                         # Category: Dyslexia
│   ├── _index.md
│   └── what-is-dyslexia.md
├── get-the-most-out-of-bookbot/                      # Category: Tips for Parents
│   ├── _index.md
│   └── 8-ways-to-boost-reading-motivation.md
├── how-do-children-learn-to-read/                     # Category: Science of Reading
│   ├── _index.md
│   └── what-is-phonics.md
└── bookbot-wins-global-edtech-competition.md          # Root-level post (no category)
```

**This is NOT a page bundle system.** Articles are standalone `.md` files. Images are stored separately in `/assets/updates/`.

### Existing Categories

| Folder | Display Name | Description |
|--------|-------------|-------------|
| `dyslexia/` | Dyslexia | Signs, support, strategies for dyslexic learners |
| `get-the-most-out-of-bookbot/` | Tips for Parents | Motivation, learning techniques, Bookbot usage |
| `how-do-children-learn-to-read/` | Science of Reading | Phonics, decoding, fluency, literacy instruction |

Research-backed articles will typically go in `how-do-children-learn-to-read/`. Bookbot tips go in `get-the-most-out-of-bookbot/`. Use the root level for announcements, awards, and topics that don't fit a category.

---

## Image Asset Structure

Images are NOT stored alongside the article. They go in `/assets/updates/`:

```
assets/updates/
├── what-is-phonics.png              # Hero image for that article
├── children-reading-books-outdoors-group.jpeg   # Inline image
├── synthetic-and-analytic-phonics.jpeg          # Inline image
└── ...
```

Hugo's render-image hook (`layouts/_default/_markup/render-image.html`) handles all processing:
- Routes `/images/updates/*` to `/assets/updates/`
- Converts to WebP at quality 60
- Generates responsive srcset at widths: 480, 768, 1024, 1280px
- Adds lazy loading by default (hero images loaded eagerly via template)

---

## YAML Front Matter

Every article must begin with this YAML front matter:

```yaml
---
title: "Phonemic Awareness Activities That Build Strong Readers"
heading: "Fun Phonemic Awareness Activities for Grades K–3"
slug: "phonemic-awareness-activities"
date: YYYY-MM-DD
image: "/images/updates/hero-image-name.png"
author: "$BOOKBOT_AUTHOR"
category: "how-do-children-learn-to-read"
description: "Compelling meta description for SEO and card previews."
sitemap:
  priority: 0.5
  changefreq: "monthly"
---
```

### Field Rules

| Field | Required | Notes |
|-------|----------|-------|
| `title` | Yes | SEO title (browser tab, search results). 50–60 characters. In quotes. See **Title & Heading Guidelines** below. |
| `heading` | Yes | Display heading on the page. Same intent as `title` but not identical. See **Title & Heading Guidelines** below. |
| `slug` | Yes | URL-friendly identifier based on the primary keyword. Lowercase, hyphenated. Must match the filename (without `.md`). See **Slug Convention** below. |
| `date` | Yes | ISO format: `YYYY-MM-DD`. Use the publication/generation date. |
| `image` | Yes | Hero image path: `/images/updates/[name].png`. This path maps to `/assets/updates/[name].png`. |
| `author` | Yes | Use the value of `$BOOKBOT_AUTHOR` env var (set during `/setup`). Links to `/content/authors/[author].md`. |
| `category` | Conditional | Required if the post lives inside a subcategory folder. Value must match the folder name exactly (e.g., `"how-do-children-learn-to-read"`). Omit for root-level posts. |
| `description` | Yes | Meta description for SEO and card preview. Compelling, not clickbait. Aim for 150–160 characters. |
| `sitemap.priority` | Yes | Always `0.5` for individual posts. |
| `sitemap.changefreq` | Yes | Always `"monthly"` for individual posts. |

### Title & Heading Guidelines

The `title` (SEO title) and `heading` (H1 displayed on the page) must be crafted deliberately. The H1 in the article body (`# ...`) should match the `heading` field.

**Audience:** Parents and teachers of children ages 5–9 (Grades K–3). Use American English spelling. Do not include any brand name in either field.

**Inputs to consider:**
- The article's primary keyword
- Any secondary keywords (up to 5)
- The article summary/outline
- The article format (how-to, guide, checklist, explainer, comparison, etc.)
- The key takeaway or promise of the article

#### Title rules (`title` field — the SEO title)
- Front-load the main topic — put the primary keyword near the start
- Aim for 50–60 characters; keep it concise and specific
- Include the primary keyword (or a close variant) exactly once
- Use secondary keywords only if they fit naturally — never keyword-stuff
- Avoid filler words: "ultimate," "best," "complete," "everything you need" — unless the article genuinely delivers on that promise
- No quotes, no ALL CAPS

#### Heading rules (`heading` field — the display H1)
- Human-friendly and clear; can be slightly longer than the title
- Include the primary keyword (or a close variant) exactly once
- Must express the same intent as the title, but not be identical to it
- If useful, include a parent/teacher framing like "for Grades K–3" or "for Ages 5–9," but only when it reads naturally

#### Example

For an article about phonemic awareness activities:

```yaml
title: "Phonemic Awareness Activities That Build Strong Readers"
heading: "Fun Phonemic Awareness Activities for Grades K–3"
```

### Fields NOT Used

The existing site does **not** use these fields in front matter — do not include them:
- `tags`
- `excerpt`
- `seo_keywords`
- `reading_time`
- `draft`

---

## Article Body Format

After the front matter, the article body is standard Markdown starting with an H1 title.

```markdown
# [Article Title]

[Opening paragraphs — the hook]

![Descriptive alt text](/images/updates/image-name.png)

[Continue article body with ## subheadings, inline images throughout]

## Section Heading

[Content with images interspersed naturally]

---

**References**

[Citations with links]
```

### Heading Hierarchy
- `#` (H1): Article title — used once at the top of the body
- `##` (H2): Major sections
- `###` (H3): Subsections (use sparingly)
- Never skip heading levels

### Images in Body

Reference images using the `/images/updates/` path prefix:

```markdown
![Descriptive alt text](/images/updates/image-name.png)
```

Hugo's render hook automatically:
1. Strips the `/images/` prefix
2. Finds the file in `/assets/updates/`
3. Generates responsive WebP versions with srcset
4. Adds lazy loading

**Images are expected throughout the article** — not just at the top. Looking at existing posts, articles typically include 3–8+ images interspersed between paragraphs to break up text and illustrate concepts.

### Internal Links
- Link to other updates: `[related article](/updates/slug/)` or `[related article](/updates/category/slug/)`
- Link to author pages: `[author name](/author/author-slug)`

---

## References Section Format

The existing site uses a simple bold heading (not an H2) with linked references:

```markdown
**References**

[Author, A. B. (Year). Article title](https://url-here)

[Author, C. D., & Author, E. F. (Year). Article title. *Journal Name*](https://doi-url-here)
```

Or as plain text with inline links:

```markdown
**References**

Author, A. B., Author, C. D., & Author, E. F. (Year). Article title. *Journal Name*, Volume(Issue). https://doi.org/xxxxx

Author, G. H. (Year). Article title. *Journal Name*. https://full-url-here
```

- Use APA 7th edition format
- Include DOI or full URL for every reference
- Italicize journal names using `*Journal Name*`
- List alphabetically by first author's last name

---

## File Naming Conventions

### Article Files
- Lowercase, hyphenated, descriptive
- Filename must match the `slug` in front matter
- Examples:
  - `should-child-use-ai-homework.md`
  - `phonemic-awareness-matters.md`
  - `what-is-phonics.md`

### Image Files
- Stored in `/assets/updates/`
- Lowercase, hyphenated, descriptive names
- Format: PNG or JPEG (Hugo auto-converts to WebP for delivery)
- Examples:
  - `should-child-use-ai-homework.png` (hero)
  - `child-reading-with-parent.jpeg` (inline)
  - `phonics-comparison-diagram.png` (inline)

### Slug Convention
- Derive the slug from the **primary keyword**, not the article title
- Lowercase, hyphenated
- Keep it short — aim for 2–4 words (3–5 URL segments max)
- Add a qualifier only when needed for clarity or uniqueness (e.g., `phonemic-awareness-activities` rather than just `phonemic-awareness` if there's already a general article on that topic)
- Drop stop words (a, an, the, is, for, of) unless removing them changes the meaning
- Examples:
  - Primary keyword "phonemic awareness activities" → `phonemic-awareness-activities`
  - Primary keyword "what is phonics" → `what-is-phonics`
  - Primary keyword "reading motivation" → `reading-motivation`
  - Primary keyword "AI for homework" → `ai-for-homework`

---

## Author Setup

Each author needs a profile at `/content/authors/[author-slug].md` and a photo at `/assets/authors/[author-slug].jpeg` (or `.png`).

Known authors: `adrian-dewitts`, `belinda-moore`, `debs-moir`, `jenni-mills`, `shelly-dollosa`, `sanchari`.

If the `$BOOKBOT_AUTHOR` doesn't have an author file yet, create one as part of the first article publish. Reference existing author files for the exact format.

---

## Process Documentation

Process documentation files are NOT published to the Hugo site. They are stored in the working directory for internal reference:

```
[slug]-process-documentation.md
```

---

## Publishing & Deployment

- **Repository:** `bookbot-kids/bookbot-www` (published via GitHub API — no local clone needed)
- **Production:** Merges to `main` branch trigger auto-deploy to https://www.bookbotkids.com/
- **Preview:** Branch deploys generate preview URLs
- **Build command:** `hugo --minify`
- **Build output:** `public/`

### File Placement on Publish

When publishing a new article (via GitHub API):

```
bookbot-www/
├── assets/updates/
│   ├── [hero-image].png           # Hero image
│   ├── [inline-image-1].jpeg      # Inline images
│   └── [inline-image-2].png
└── content/updates/
    └── [category-if-applicable]/
        └── [slug].md              # Article file
```
