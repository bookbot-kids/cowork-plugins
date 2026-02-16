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

Sanchari's research-backed articles will typically go in `how-do-children-learn-to-read/` or at the root level, depending on topic fit.

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
title: "Article Title Here"
heading: "Article Title Here"
slug: "article-slug-here"
date: YYYY-MM-DD
image: "/images/updates/hero-image-name.png"
author: "sanchari"
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
| `title` | Yes | Page title (used in SEO, browser tab). Under 70 characters. In quotes. |
| `heading` | Yes | Display heading on the page. Usually matches `title`, but can differ. |
| `slug` | Yes | URL-friendly identifier. Lowercase, hyphenated. Must match the filename (without `.md`). |
| `date` | Yes | ISO format: `YYYY-MM-DD`. Use the publication/generation date. |
| `image` | Yes | Hero image path: `/images/updates/[name].png`. This path maps to `/assets/updates/[name].png`. |
| `author` | Yes | Author slug in kebab-case. For Sanchari, use `"sanchari"`. Links to `/content/authors/sanchari.md`. |
| `category` | Conditional | Required if the post lives inside a subcategory folder. Value must match the folder name exactly (e.g., `"how-do-children-learn-to-read"`). Omit for root-level posts. |
| `description` | Yes | Meta description for SEO and card preview. Compelling, not clickbait. Aim for 150–160 characters. |
| `sitemap.priority` | Yes | Always `0.5` for individual posts. |
| `sitemap.changefreq` | Yes | Always `"monthly"` for individual posts. |

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
- Lowercase, hyphenated
- Derived from the article title
- Remove articles (a, an, the) and filler words if slug exceeds ~6 segments
- Examples:
  - "Should My Child Use AI for Homework?" → `should-child-use-ai-homework`
  - "What is Phonics? Synthetic vs Analytic Explained" → `what-is-phonics`
  - "8 Ways to Boost Reading Motivation" → `8-ways-to-boost-reading-motivation`

---

## Author Setup

Sanchari needs an author profile at `/content/authors/sanchari.md` and a photo at `/assets/authors/sanchari.jpeg` (or `.png`). The author file format matches existing authors on the site.

If the author file doesn't exist yet, create it as part of the first article publish. Reference existing author files (e.g., `adrian-dewitts.md`, `belinda-moore.md`) for the exact format.

---

## Process Documentation

Process documentation files are NOT published to the Hugo site. They are stored in the working directory for internal reference:

```
[slug]-process-documentation.md
```

---

## Publishing & Deployment

- **Repository:** `bookbot-www` (the Hugo site repo)
- **Production:** Pushes to `main` branch trigger auto-deploy to https://www.bookbotkids.com/
- **Preview:** Branch deploys generate preview URLs
- **Build command:** `hugo --minify`
- **Build output:** `public/`

### File Placement on Publish

When committing a new article:

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
