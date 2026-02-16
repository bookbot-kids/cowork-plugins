# Image Generation Guidelines

This skill defines how to generate images for Bookbot blog posts using the `gpt-image-1` MCP server. Images are generated after the article is written (Phase 3.5 of the workflow) so visual content aligns with the article's themes.

---

## Image Philosophy

Looking at existing Bookbot articles, **images are used generously throughout** — not just as a hero at the top. A typical article has 3–8+ images interspersed between paragraphs to break up text, illustrate concepts, and maintain visual engagement. This is a key part of the Bookbot content style.

---

## What to Generate

Every article gets:
1. **Hero image** (required) — the featured image shown in the article header and social sharing
2. **Inline images** (required, 3–6) — placed throughout the article body between paragraphs

### Types of Inline Images
Based on existing articles, inline images serve several purposes:
- **Scene-setting photos** — children reading, parents with kids, classroom moments
- **Diagrams and infographics** — comparing concepts, showing processes
- **Section heading illustrations** — visual breaks that introduce a new topic
- **Concept illustrations** — making abstract ideas (like brain development or phonics) visual

---

## Hero Image Specifications

| Property | Value |
|----------|-------|
| Aspect ratio | 3:2 |
| Dimensions | 1536 x 1024 px |
| Format | PNG |
| File path | `/assets/updates/[slug].png` |
| Front matter reference | `/images/updates/[slug].png` |

The hero image filename should match the article slug (e.g., article `should-child-use-ai-homework.md` → hero `should-child-use-ai-homework.png`).

Hugo processes the hero image eagerly (no lazy load) at full viewport width with responsive srcset.

---

## Inline Image Specifications

| Property | Value |
|----------|-------|
| Aspect ratio | Flexible — 2:1 for scene photos, variable for diagrams |
| Dimensions | Minimum 1156px wide. Typical: 1156–1728px wide. |
| Format | PNG or JPEG |
| File path | `/assets/updates/[descriptive-name].png` or `.jpeg` |
| Markdown reference | `![Alt text](/images/updates/[descriptive-name].png)` |

Hugo auto-converts all inline images to WebP at quality 60 and generates responsive srcset at 480, 768, 1024, 1280px widths. Lazy loading is applied automatically.

---

## Visual Style Guide — Bookbot Brand

### Overall Aesthetic
- Warm, inviting, educational
- Modern and clean — not clinical or sterile
- Diverse representation of children, families, and educators
- Natural lighting preferred over studio lighting
- Soft color palette: warm yellows, greens, blues — avoid harsh neons

### Subject Matter Guidelines
- Children engaged in reading or learning activities
- Parent-child reading moments
- Classrooms and learning environments
- Abstract representations of brain development or reading science
- Books, letters, and literacy-related objects
- Infographics and diagrams for comparing concepts

### Style Keywords for Prompts
Include these in every image prompt:
- "warm lighting"
- "educational"
- "diverse children"
- "modern illustration" OR "photorealistic" (choose based on article tone)
- "warm, inviting palette"

### Things to Avoid in Images
- Stock photo cliches (pointing at screens, thumbs up, etc.)
- Overly corporate or sterile environments
- Images that could be frightening or anxiety-inducing for parents
- Text or words embedded in the image (these don't render well in AI generation)
- Logos or brand marks (Bookbot logo is in the site template)
- Stereotypical gender roles in learning contexts
- Children looking distressed or struggling (show engagement, not frustration)

---

## Prompt Templates

### Hero Image Prompt Template
```
A [style: warm illustration / photorealistic photo] of [subject derived from article topic].
[Specific scene description tied to the article's main theme].
Warm lighting, educational setting, diverse representation.
Modern, clean composition. No text or logos in the image.
Dimensions: 1536x1024. High quality.
```

### Inline Scene Image Prompt Template
```
A [style] of [specific scene from the article section].
[Description of the moment or concept being illustrated].
Warm, natural lighting. Diverse representation.
Clean composition. No text or logos.
Wide format, minimum 1200px wide. High quality.
```

### Inline Diagram/Infographic Prompt Template
```
A clean, modern infographic showing [concept from article].
[Specific elements to include: labels, comparisons, flow].
Warm educational color palette. Clear visual hierarchy.
Simple and readable. No small text that would be illegible.
Wide format. High quality.
```

### Example Prompts

**Hero image — article about AI in education:**
```
A warm photorealistic photo of a child sitting at a desk with a tablet,
engaged in a reading activity with a parent nearby offering guidance.
Warm natural lighting, modern home setting, diverse family.
Clean composition showing the balance between technology and human connection.
No text or logos. Dimensions: 1536x1024. High quality.
```

**Inline scene — children reading outdoors:**
```
A warm photorealistic photo of a group of diverse children sitting outdoors
reading books together, with natural sunlight filtering through trees.
Relaxed, joyful atmosphere. Clean composition.
No text or logos. Wide format. High quality.
```

**Inline diagram — comparing two approaches:**
```
A clean, modern infographic comparing two teaching methods side by side.
Left column labeled with key features of one approach,
right column with the other. Connected by arrows showing relationships.
Warm educational color palette with soft blues and greens.
Clear visual hierarchy, easy to read at a glance.
Wide format. High quality.
```

---

## Alt Text Format

Every image must have descriptive alt text. Follow the pattern used in existing articles:

```markdown
![Descriptive text explaining what the image shows](/images/updates/filename.png)
```

### Examples from Existing Articles
- `Young child sitting on a couch reading a book with colorful pictures`
- `Diagram showing the two main types of phonics: Synthetic Phonics and Analytic Phonics`
- `Group of five children sitting outdoors reading books together under a tree`
- `Infographic comparing Analytic Phonics versus Synthetic Phonics teaching methods, showing differences in letter-sound instruction, word focus, decoding strategies, and book types`
- `The Five Keys to Reading infographic showing Comprehension, Fluency, Vocabulary, Phonics, and Phonemic Awareness as essential components of literacy`

### Alt Text Rules
- Describe what's in the image specifically
- For infographics/diagrams, describe the content and structure
- Keep concise but complete
- Never start with "Image of..." or "Photo of..."
- Include relevant context for screen reader users

---

## MCP Server Call Format

When using the `gpt-image-1` MCP server, call the image generation tool with:

```json
{
  "prompt": "[constructed prompt from template above]",
  "size": "1536x1024",
  "quality": "high"
}
```

For inline images that need different dimensions:
```json
{
  "prompt": "[constructed prompt]",
  "size": "1536x1024",
  "quality": "high"
}
```

Note: `gpt-image-1` may use different parameter names. Adjust based on the MCP server's actual tool schema. The key parameters to set are:
- Prompt (the image description)
- Size / dimensions (1536x1024 for hero, at least 1200px wide for inline)
- Quality (high)

**Output format:** Save generated images as PNG files in `/assets/updates/`. Hugo handles the WebP conversion.

---

## File Naming

| Image Type | Naming Convention | Example |
|-----------|------------------|---------|
| Hero | Match the article slug | `should-child-use-ai-homework.png` |
| Inline scene | Descriptive, hyphenated | `child-reading-with-parent.jpeg` |
| Inline diagram | Concept-based, hyphenated | `synthetic-and-analytic-phonics.jpeg` |
| Section heading | Short, topic-based | `intrinsic-motivation.jpeg` |

All image files go in `/assets/updates/` — NOT alongside the article.

---

## Handling Failed Generations

If an image generation fails or produces poor results:

1. **Retry once** with a refined prompt (add more specific details, simplify composition)
2. **Try alternative style** — switch between "photorealistic" and "modern illustration"
3. **If still failing** — note the failure in the review conversation and proceed. Mark the image placement in the article with a TODO comment for manual addition later.

Do not block article publication on individual inline image failures. The hero image is required.

---

## Image Review

Present all generated images to the reviewer in the conversation:
- Show each image
- Display the prompt used
- Display the alt text
- Show where it appears in the article

Images can be regenerated with modified prompts based on reviewer feedback.
