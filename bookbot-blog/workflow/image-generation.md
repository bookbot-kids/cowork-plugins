# Image Generation Guidelines

This guide defines how to generate images for Bookbot blog posts using the **Codex CLI's `@imagegen` skill** (`gpt-image-2`). Images are generated after the article is written (Phase 3.5 of the workflow) so visual content aligns with the article's themes.

> **Image engine:** All generation runs through Codex `@imagegen`, which authenticates against the local ChatGPT plan at `~/.codex/auth.json`. There is **no API key** and **no per-image charge**.
>
> **Hard restriction — never call the raw image API.** Generation must go **only** through Codex `@imagegen`. Never call the OpenAI gpt-image API directly, never use an `OPENAI_API_KEY`, and never invoke Codex's "transparency fallback" to `gpt-image-1.5`. If `@imagegen` fails, retry the prompt (see "Handling Failed Generations") — do not fall back to a direct API.

---

## Image Philosophy

Looking at existing Bookbot articles, **images are used generously throughout** — not just as a hero at the top. A typical article has 3–8+ images interspersed between paragraphs to break up text, illustrate concepts, and maintain visual engagement. This is a key part of the Bookbot content style.

---

## What to Generate

Every article gets:
1. **Hero image** (required) — photographic, the featured image shown in the article header and social sharing
2. **Inline images** (required, 3–6) — placed throughout the article body between paragraphs, either photographic or illustration

### Types of Inline Images

Inline images are **illustrations by default**. Only use photographic inline images in exceptional cases where a real scene is genuinely more effective than an illustration (e.g., a photo of a specific classroom environment the article is about).

Illustrations serve most purposes:
- **Diagrams and infographics** — comparing concepts, showing processes, side-by-side comparisons
- **Section heading illustrations** — visual breaks that introduce a new topic
- **Concept illustrations** — making abstract ideas (like brain development or phonics) visual
- **Step-by-step visuals** — numbered flows, checklists, timelines

### Choosing Photographic vs. Illustration

- **Hero only → Photographic**: The hero image is always a warm, editorial photo.
- **Inline → Illustration (default)**: Use the Bookbot flat infographic style for virtually all inline images. Reserve photographic inline images for cases where a real scene adds something an illustration cannot.

---

## Category Theming (required for every image)

Every image — hero and inline, photo and infographic — must be **anchored to the article's content category**. Pick the one category the article belongs to and let it drive the concept:

| Category | Visual angle to lean into |
|----------|---------------------------|
| **Dyslexia** | Supportive, confidence-building moments; multi-sensory learning; never distress or struggle |
| **EdTech** | Children/families using the Bookbot app or a tablet; the blend of technology and human guidance |
| **Science of Reading** | Phonics, decoding, fluency, the building blocks of literacy; classroom/teaching moments |
| **Tips for Parents** | Warm parent–child reading and motivation moments at home |

The category shapes the *subject and scene* of the image — **do not render the category name as visible text or a badge** on the image. The visual should simply read as belonging to that category.

---

## Hero Image Specifications

| Property | Value |
|----------|-------|
| Style | Photographic |
| Aspect ratio | 16:9 |
| Resolution | 2K |
| Format | PNG |
| File path | `/assets/updates/[slug].png` |
| Front matter reference | `/images/updates/[slug].png` |

The hero image filename should match the article slug (e.g., article `should-child-use-ai-homework.md` → hero `should-child-use-ai-homework.png`).

Hugo processes the hero image eagerly (no lazy load) at full viewport width with responsive srcset.

---

## Inline Image Specifications

| Property | Value |
|----------|-------|
| Style | Illustration (default); photographic only in exceptional cases |
| Aspect ratio | **Choose based on content** — see aspect ratio guide below |
| Resolution | 2K |
| Format | PNG |
| File path | `/assets/updates/[descriptive-name].png` |
| Markdown reference | `![Alt text](/images/updates/[descriptive-name].png)` |

### Illustration Aspect Ratio Guide

Choose the aspect ratio that best fits the diagram's content — do not default to portrait for every illustration:

| Aspect Ratio | When to use |
|---|---|
| **16:9 or 21:9** (landscape) | Wide comparisons, timelines, multi-column layouts, horizontal flows |
| **4:3 or 3:2** (landscape) | Side-by-side comparisons, broad infographics, diagrams with moderate width |
| **1:1** (square) | Balanced concepts, single-focus diagrams, circular or symmetrical layouts |
| **4:5 or 3:4** (portrait) | Vertical flows, step-by-step sequences, tall hierarchies or lists |
| **2:3 or 9:16** (portrait) | Very tall diagrams, narrow step sequences — use sparingly |

Think about the shape of the content first, then pick the ratio that gives it the most natural room.

Hugo auto-converts all inline images to WebP at quality 60 and generates responsive srcset at 480, 768, 1024, 1280px widths. Lazy loading is applied automatically.

---

## Visual Style Guide — Photographic Images

### Reference Photo (required)

Every photographic image **must attach one real Bookbot photo** from `references/photos/` (via `-i`, see "Codex Image Generation" below) as a subject/style anchor. These are authentic photos of real, diverse children using Bookbot in warm, natural home settings — they keep the AI photos grounded and on-brand. Choose the one whose subject best fits the scene:

| File | Subject |
|------|---------|
| `references/photos/ren.jpg` | A young boy relaxed on a couch reading on a tablet |
| `references/photos/family.jpg` | Two children at a table using a tablet with an adult guiding |
| `references/photos/family2.jpg` | A family reading moment |
| `references/photos/forester&ren.jpg` | Two children together with books/tablet |
| `references/photos/edith-speak.jpg` | A child speaking/reading aloud |

Match the people and warmth of the reference, but invent a fresh scene tied to the article — do not copy the reference composition.

### Overall Aesthetic
- **Photorealistic** — must look like a real photograph, not AI-generated or illustrated
- Warm, inviting, editorial photography
- Modern and clean — not clinical or sterile
- Diverse representation of children, families, and educators
- Natural lighting preferred over studio lighting
- Soft color palette: warm yellows, greens, blues — avoid harsh neons

### Subject Matter Guidelines

**Be creative and specific to the article topic.** Do not default to a generic "child reading a book" scene — every article deserves a distinct, imaginative image that reflects its specific theme.

Think about what would make someone stop scrolling. Consider:
- The article's core concept or surprising finding — can it be shown as a scene?
- Unexpected but relevant settings (a library, an outdoor market, a kitchen table, a museum, a park)
- Moments of discovery, curiosity, or connection — not just sitting and reading
- Close-ups of hands, eyes, objects, or details that evoke the article's theme
- Environments or situations that haven't been used in recent articles

**Subject ideas (vary across articles):**
- A child absorbed in a book in an unusual or visually rich location
- An educator in a dynamic classroom moment
- A parent-child interaction tied to the article's specific theme
- Literacy-related objects, environments, or textures used creatively
- Abstract representations of the article's concept through a real scene

Avoid repeating the same basic composition (e.g., a child sitting at a desk or table reading) across multiple articles.

### Style Keywords for Photographic Prompts
Include these in every photographic image prompt:
- "warm lighting"
- "educational"
- "diverse"
- "photorealistic"
- "warm, inviting palette"
- "looks like a real photograph"

---

## Visual Style Guide — Illustrations

Illustrations must match the Bookbot flat infographic style. Three brand reference images **must be attached** to the Codex call (via `-i`, see "Codex Image Generation" below) so `gpt-image-2` matches the brand precisely:

| Reference | Path | Purpose |
|-----------|------|---------|
| Style + colours | `references/reference-illustration.webp` | The authoritative flat-infographic style and brand colour palette |
| Logo | `references/logo.png` | The Bookbot wordmark, for the footer credit area |
| Icon | `references/yellow1.1.png` | The Bookbot icon/mascot mark |

### Style Description
- Flat educational infographic style
- Bold geometric shapes (triangles, rectangles) used as structural metaphors
- Solid saturated color fills — orange, cyan, red, green, yellow, purple
- Thick black outlines around all shapes
- Clean bold sans-serif text labels centered in shapes
- Solid brand background color (choose one randomly per image — see palette below)
- No gradients, no shadows, no 3D effects
- Diagrammatic and clear, designed for parent/teacher audiences

### Background Color Palette

For each illustration, randomly select one background color from this brand palette:

| Name | Hex |
|------|-----|
| Pale yellow | `#FFF4A8` |
| Mint green | `#C3EFD0` |
| Soft peach | `#FDC1B0` |
| Sky blue | `#A8ECFF` |
| Lavender | `#D6BAFF` |

Pick a different color for each illustration in the article so they vary visually.

### Style Keywords for Illustration Prompts
Include these in every illustration prompt:
- "flat infographic style"
- "bold geometric shapes"
- "solid color fills with black outlines"
- "educational diagram"
- "[chosen background color] solid background" (e.g., "pale yellow #FFF4A8 solid background")
- "clean sans-serif text labels"

---

## Things to Avoid in All Images
- Stock photo cliches (pointing at screens, thumbs up, etc.)
- Overly corporate or sterile environments
- Images that could be frightening or anxiety-inducing for parents
- Logos or brand marks (Bookbot logo is in the site template)
- Stereotypical gender roles in learning contexts
- Children looking distressed or struggling (show engagement, not frustration)

---

## Prompt Templates

**Write very descriptive prompts.** `gpt-image-2` rewards detail — name the subject, the setting, the lighting, the composition, the colours, the mood, and (for infographics) every label and shape. Vague prompts produce generic images. Each prompt must also state the **category theme** (see "Category Theming" above) so the scene clearly belongs to that pillar.

### Hero Image (Photographic, 16:9)
```
A warm, photorealistic editorial photo for a [CATEGORY] article about [topic].
Subject: [specific people derived from the attached reference photo — diverse children/family/educator].
Scene: [detailed setting tied to the article's main theme — location, props, action].
Lighting: warm, natural light; soft, inviting palette of yellows, greens, blues.
Composition: modern and clean, [framing — close-up / wide / over-the-shoulder].
Mood: curiosity, connection, engagement (never distress).
Looks like a real photograph. No text, no logos, no badges in the image. High quality.
```

### Inline Photographic Image
```
A warm, photorealistic photo for a [CATEGORY] article, illustrating [specific moment from this article section].
Subject and scene: [detailed description of the people, setting, and action], consistent with the attached reference photo.
Warm, natural lighting. Diverse representation. [Framing.]
Looks like a real photograph. No text, no logos. High quality.
```

### Inline Illustration (with brand references)
```
A flat educational infographic in the exact visual style and brand colours of the attached reference illustration, for a [CATEGORY] article.
Concept: [concept from this article section].
Layout: [every element spelled out — sections, labels, comparisons, arrows, numbered steps].
Bold geometric shapes with solid colour fills and thick black outlines.
Clean bold sans-serif text labels, spelled correctly: [list the exact label text].
[Chosen background colour, e.g. "Pale yellow #FFF4A8"] solid background.
No gradients, no shadows, no 3D. Clear, readable, diagrammatic.
```

Pick a different background color from the brand palette for each illustration in the article.

### Example Prompts

**Hero image — EdTech article about AI in education** (attach `references/photos/family.jpg`):
```
A warm, photorealistic editorial photo for an EdTech article about balancing AI tools with parent guidance.
Subject: a diverse mother and her two primary-school children, like the people in the attached reference photo.
Scene: at a sunlit kitchen table, one child reading aloud from a tablet running a reading app while the parent listens and points encouragingly; a few picture books stacked beside them.
Lighting: warm morning light through a window; soft yellows and greens.
Composition: clean over-the-shoulder framing showing both the screen and the parent's attentive face.
Mood: curiosity and connection. Looks like a real photograph. No text, no logos, no badges. High quality.
```

**Inline photo — Tips for Parents, reading outdoors** (attach `references/photos/forester&ren.jpg`):
```
A warm, photorealistic photo for a Tips for Parents article about making reading joyful.
Subject and scene: two diverse children, like those in the attached reference photo, sitting together on a picnic blanket in a leafy park, sharing a picture book and laughing, dappled natural sunlight through trees.
Warm, natural lighting. Relaxed, joyful atmosphere. Clean composition.
Looks like a real photograph. No text, no logos. High quality.
```

**Inline illustration — Science of Reading, comparing two approaches** (attach `references/reference-illustration.webp`, `references/logo.png`, `references/yellow1.1.png`):
```
A flat educational infographic in the exact visual style and brand colours of the attached reference illustration, for a Science of Reading article.
Concept: synthetic phonics vs analytic phonics, side by side.
Layout: two columns. Left column header "Synthetic Phonics" with three short bullet shapes; right column header "Analytic Phonics" with three short bullet shapes; a clear vertical divider between them.
Bold geometric shapes with solid colour fills and thick black outlines.
Clean bold sans-serif labels spelled correctly: "Synthetic Phonics", "Analytic Phonics".
Sky blue #A8ECFF solid background. No gradients, no shadows, no 3D. Clear and readable.
```

---

## Codex Image Generation

All images are generated by invoking the Codex CLI's `@imagegen` skill non-interactively with `codex exec`. Codex runs `gpt-image-2` against the local ChatGPT-plan auth (`~/.codex/auth.json`) — no API key, no per-image charge. Reference images are attached with `-i` (comma-separated) and also named in the prompt so the model conditions on them. Tell Codex to save the final PNG to the absolute target path and to print `SAVED <path>` so you can confirm.

**Aspect ratio** is expressed in words inside the prompt (e.g. "wide 16:9 landscape", "square 1:1", "tall 4:5 portrait") — there is no `aspectRatio` flag. Pick the ratio per the "Illustration Aspect Ratio Guide"; hero is always "wide 16:9 landscape".

> Run these from the working directory (where `assets/updates/` lives). `$REF` below is the path to the plugin's `references/` folder.

### Photographic Images (hero + photographic inline)

Attach the chosen reference photo from `references/photos/`:

```bash
codex exec --skip-git-repo-check --sandbox danger-full-access \
  --cd "$PWD" --add-dir "$PWD/assets/updates" \
  -i "$REF/photos/family.jpg" \
  --output-last-message /tmp/imagegen-last.txt \
  "@imagegen PROMPT_HERE. Use the attached photo as the subject and style reference (real, diverse children in a warm natural setting) but invent a fresh scene. Composition: wide 16:9 landscape. Looks like a real photograph, no text, no logos. Save the final PNG to $PWD/assets/updates/SLUG.png and then print exactly: SAVED $PWD/assets/updates/SLUG.png"
```

### Illustration Images (with brand references)

Attach all three brand references so the style, colours, logo, and icon match:

```bash
codex exec --skip-git-repo-check --sandbox danger-full-access \
  --cd "$PWD" --add-dir "$PWD/assets/updates" \
  -i "$REF/reference-illustration.webp,$REF/logo.png,$REF/yellow1.1.png" \
  --output-last-message /tmp/imagegen-last.txt \
  "@imagegen PROMPT_HERE. Match the exact flat-infographic style and brand colours of the attached reference illustration. Use the attached Bookbot logo and icon for branding only if a small footer credit is appropriate. Composition: [aspect ratio in words]. Save the final PNG to $PWD/assets/updates/NAME.png and then print exactly: SAVED $PWD/assets/updates/NAME.png"
```

### Illustration Translation (Spanish)

To produce a Spanish version of an illustration, attach the **original English illustration** and have `@imagegen` edit it — replacing only the text labels while preserving the exact layout, colours, shapes, and style.

1. Read the original English illustration to identify all text labels in the image
2. Translate each label to Latin American Spanish following the vocabulary rules in `spanish-translation.md` (e.g., "educadores" → "maestros", "Reading Skills" → "Lectura")
3. Run the edit:
   ```bash
   codex exec --skip-git-repo-check --sandbox danger-full-access \
     --cd "$PWD" --add-dir "$PWD/assets/updates" \
     -i "$PWD/assets/updates/NAME.png" \
     --output-last-message /tmp/imagegen-last.txt \
     "@imagegen Edit the attached illustration to replace all English text with Spanish translations. Keep the exact same layout, colours, shapes, geometry, background colour, and style — change ONLY the text labels. Translations: \"English 1\" → \"Spanish 1\"; \"English 2\" → \"Spanish 2\"; \"English 3\" → \"Spanish 3\". Save the final PNG to $PWD/assets/updates/NAME-es.png and then print exactly: SAVED $PWD/assets/updates/NAME-es.png"
   ```
4. The Spanish illustration is saved with the `-es` suffix: `assets/updates/NAME-es.png`

**Important:** The Spanish version must be a visual match of the original with only the text changed.

**File naming:** Append `-es` to the original filename:
- English: `assets/updates/synthetic-vs-analytic-phonics.png`
- Spanish: `assets/updates/synthetic-vs-analytic-phonics-es.png`

**Cost:** None — billed to the ChatGPT plan, no per-image API cost.

**Hero image:** Does not need translation — photographic images contain no text labels. The hero image is shared between English and Spanish articles.

---

### Handling the Response

Codex saves the image and prints a final message. To confirm and place the file:

1. Read `/tmp/imagegen-last.txt` (or Codex's final output) and look for the `SAVED <path>` line — that is your image.
2. Verify the file exists at `assets/updates/<name>.png`.
3. **Fallback** — if Codex saved elsewhere, the newest file under `~/.codex/generated_images/` is the result; move it into `assets/updates/<name>.png`:
   ```bash
   latest=$(ls -t ~/.codex/generated_images/*/* 2>/dev/null | head -1)
   mv "$latest" "$PWD/assets/updates/NAME.png"
   ```

---

## Alt Text Format

Every image must have descriptive alt text:

```markdown
![Descriptive text explaining what the image shows](/images/updates/filename.png)
```

### Alt Text Rules
- Describe what's in the image specifically
- For infographics/diagrams, describe the content and structure
- Keep concise but complete
- Never start with "Image of..." or "Photo of..."
- Include relevant context for screen reader users

---

## File Naming

| Image Type | Naming Convention | Example |
|-----------|------------------|---------|
| Hero | Match the article slug | `should-child-use-ai-homework.png` |
| Inline photo | Descriptive, hyphenated | `child-reading-with-parent.png` |
| Inline illustration | Concept-based, hyphenated | `synthetic-vs-analytic-phonics.png` |

All image files go in `/assets/updates/` — NOT alongside the article.

---

## Handling Failed Generations

If an image generation fails or produces poor results:

1. **Retry once** with a refined prompt (add more specific details, simplify composition)
2. **Try alternative style** — for photographic, adjust the scene; for illustration, simplify the diagram
3. **If still failing** — note the failure in the review conversation and proceed. Mark the image placement in the article with a TODO comment for manual addition later.

Do not block article publication on individual inline image failures. The hero image is required.

---

## Image Review

Images are reviewed as part of the article review phase (Phase 5). During review:
- Show each image alongside the article
- If the reviewer requests changes, craft an updated prompt and regenerate the image
- Images can be regenerated as many times as needed
