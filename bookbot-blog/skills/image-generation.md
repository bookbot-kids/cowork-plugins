# Image Generation Guidelines

This skill defines how to generate images for Bookbot blog posts using the Gemini API. Images are generated after the article is written (Phase 3.5 of the workflow) so visual content aligns with the article's themes.

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
| Aspect ratio | Choose from supported values: `1:1`, `2:3`, `3:2`, `3:4`, `4:3`, `4:5`, `5:4`, `9:16`, `16:9`, `21:9` |
| Resolution | 2K |
| Format | PNG |
| File path | `/assets/updates/[descriptive-name].png` |
| Markdown reference | `![Alt text](/images/updates/[descriptive-name].png)` |

Hugo auto-converts all inline images to WebP at quality 60 and generates responsive srcset at 480, 768, 1024, 1280px widths. Lazy loading is applied automatically.

---

## Visual Style Guide — Photographic Images

### Overall Aesthetic
- Warm, inviting, editorial photography
- Modern and clean — not clinical or sterile
- Diverse representation of children, families, and educators
- Natural lighting preferred over studio lighting
- Soft color palette: warm yellows, greens, blues — avoid harsh neons

### Subject Matter Guidelines
- Children engaged in reading or learning activities
- Parent-child reading moments
- Classrooms and learning environments
- Books, letters, and literacy-related objects

### Style Keywords for Photographic Prompts
Include these in every photographic image prompt:
- "warm lighting"
- "educational"
- "diverse children"
- "photorealistic"
- "warm, inviting palette"

---

## Visual Style Guide — Illustrations

Illustrations must match the Bookbot flat infographic style. A reference image is stored at the plugin root: `references/reference-illustration.webp`. This reference image **must be included** in the API call as inline data (see API call format below) so Gemini can match the style precisely.

### Style Description
- Flat educational infographic style
- Bold geometric shapes (triangles, rectangles) used as structural metaphors
- Solid saturated color fills — orange, cyan, red, green, yellow, purple
- Thick black outlines around all shapes
- Clean bold sans-serif text labels centered in shapes
- Warm cream/beige background
- No gradients, no shadows, no 3D effects
- Diagrammatic and clear, designed for parent/teacher audiences

### Style Keywords for Illustration Prompts
Include these in every illustration prompt:
- "flat infographic style"
- "bold geometric shapes"
- "solid color fills with black outlines"
- "educational diagram"
- "warm cream background"
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

### Hero Image (Photographic, 16:9)
```
A warm photorealistic photo of [subject derived from article topic].
[Specific scene description tied to the article's main theme].
Warm lighting, educational setting, diverse representation.
Modern, clean composition. No text or logos in the image.
High quality.
```

### Inline Photographic Image
```
A warm photorealistic photo of [specific scene from the article section].
[Description of the moment or concept being illustrated].
Warm, natural lighting. Diverse representation.
Clean composition. No text or logos.
High quality.
```

### Inline Illustration (with reference image)
```
Generate an illustration in the exact same visual style as the reference image.
A flat educational infographic showing [concept from article].
[Specific elements to include: labels, comparisons, flow].
Bold geometric shapes with solid color fills and black outlines.
Clean sans-serif text labels. Warm cream background.
No gradients, no 3D. Clear and readable.
High quality.
```

### Example Prompts

**Hero image — article about AI in education:**
```
A warm photorealistic photo of a child sitting at a desk with a tablet,
engaged in a reading activity with a parent nearby offering guidance.
Warm natural lighting, modern home setting, diverse family.
Clean composition showing the balance between technology and human connection.
No text or logos. High quality.
```

**Inline photo — children reading outdoors:**
```
A warm photorealistic photo of a group of diverse children sitting outdoors
reading books together, with natural sunlight filtering through trees.
Relaxed, joyful atmosphere. Clean composition.
No text or logos. High quality.
```

**Inline illustration — comparing two approaches:**
```
Generate an illustration in the exact same visual style as the reference image.
A flat educational infographic comparing two teaching methods side by side.
Left section with key features of synthetic phonics,
right section with analytic phonics features.
Bold geometric shapes with solid color fills and black outlines.
Clean sans-serif labels. Warm cream background.
No gradients, no 3D. Clear and readable. High quality.
```

---

## Gemini API Call Format

### Photographic Images (hero + photographic inline)

```bash
curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{"parts": [{"text": "PROMPT_HERE"}]}],
    "generationConfig": {
      "responseModalities": ["IMAGE", "TEXT"],
      "imageConfig": {
        "aspectRatio": "16:9",
        "imageSize": "2K"
      }
    }
  }'
```

Adjust the `aspectRatio` for inline images. Supported values: `1:1`, `2:3`, `3:2`, `3:4`, `4:3`, `4:5`, `5:4`, `9:16`, `16:9`, `21:9`.

### Illustration Images (with style reference)

For illustrations, include the reference image as base64 inline data so Gemini matches the Bookbot style:

1. Base64-encode the reference image:
   ```bash
   base64 -w 0 references/reference-illustration.webp
   ```
   (On macOS: `base64 -i references/reference-illustration.webp`)

2. Include it in the API call:
   ```bash
   curl -s -X POST \
     "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent" \
     -H "x-goog-api-key: $GEMINI_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "contents": [{"parts": [
         {"text": "Generate an illustration in the exact same visual style as the reference image. PROMPT_HERE"},
         {"inline_data": {"mimeType": "image/webp", "data": "BASE64_ENCODED_REFERENCE_IMAGE"}}
       ]}],
       "generationConfig": {
         "responseModalities": ["IMAGE", "TEXT"],
         "imageConfig": {
           "aspectRatio": "4:3",
           "imageSize": "2K"
         }
       }
     }'
   ```

Adjust the `aspectRatio` per image. Supported values: `1:1`, `2:3`, `3:2`, `3:4`, `4:3`, `4:5`, `5:4`, `9:16`, `16:9`, `21:9`.

### Handling the Response

The API response contains the generated image as base64 in `candidates[0].content.parts`. For each image part with `inlineData`:

1. Extract the base64 string from `inlineData.data`
2. Decode and save as PNG:
   ```bash
   echo "BASE64_IMAGE_DATA" | base64 -d > /path/to/assets/updates/filename.png
   ```
3. Verify the file was saved correctly

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
