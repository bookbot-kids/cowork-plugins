# Spanish (Latin America) Translation Skill

Translate a published English blog article into Latin American Spanish, following the Bookbot voice and style guidelines below. Produce a complete, publication-ready `.md` file.

---

## Audience

- Parents in Latin America (Mexico, Colombia, Argentina, Chile, Peru, etc.)
- They're browsing on their phones, deciding whether to trust Bookbot with their child's reading
- They need to feel like they're talking to a knowledgeable friend, not reading a corporate brochure

---

## Core Style Principles

### 1. Talk to the parent directly

Use "tu hijo" instead of impersonal constructs. The parent is the reader — speak to them.

| Don't write | Write instead |
|---|---|
| Los jóvenes lectores pueden practicar... | Tu hijo puede practicar... |
| Accede a libros y audiolibros | Tu hijo puede leer y escuchar libros |
| El niño desarrolla habilidades | Tu hijo aprende más rápido |
| Los estudiantes muestran mejoras | Los niños mejoran |

### 2. Use active voice, not passive

Passive voice sounds robotic in Spanish. Flip it.

| Don't write | Write instead |
|---|---|
| Tu suscripción ha sido recibida | Ya estás suscrito |
| Los libros son calificados por niños | Los niños califican los libros |
| Se ha enviado un correo electrónico | Te enviamos un correo |
| La lectura es practicada diariamente | Tu hijo practica la lectura cada día |

### 3. Choose everyday words over formal ones

Latin American parents say "maestros", not "educadores". They say "ayuda", not "retroalimentación".

| Formal / textbook | Natural LatAm |
|---|---|
| educadores | maestros |
| retroalimentación | ayuda / respuestas |
| brindar | dar |
| alfabetización | lectura |
| aplicación | app |
| proporcionar | dar |
| empoderar | ayudar / darle fuerza |
| habilidades de lectura | lectura |
| lectores reacios | niños que no quieren leer |
| vida silvestre | animales salvajes |
| materiales de lectura | libros |
| recompensas | premios |
| artículos de soporte | artículos de ayuda |
| pequeñas empresas | pequeños negocios |
| exámenes estandarizados | exámenes |

### 4. Keep it short

If you can say it in one sentence, don't use three. Cut filler words and corporate padding.

| Don't write | Write instead |
|---|---|
| Nos apasiona asegurar que la lectura en inglés sea accesible para cada niño que está aprendiendo el idioma. | Queremos que cada niño tenga acceso a buenos libros en inglés. |
| Bookbot facilita a padres y educadores superar los obstáculos de enseñar fonética. | Con Bookbot, enseñar fonética es fácil para padres y maestros. |
| Domina las habilidades de lectura y crea un lector feliz y seguro. | Que tu hijo aprenda a leer feliz y con confianza. |

### 5. Avoid "translationese"

Don't follow English word order or phrasing. Rewrite the idea in Spanish as a native speaker would say it.

| English source | Bad translation | Good translation |
|---|---|---|
| Proven Reading Results Kids Love | Resultados de Lectura Comprobados que Encantan a los Niños | Resultados reales que a los niños les encantan |
| Learning to Love Reading | Aprendiendo a amar la lectura | Aprender a amar la lectura |
| For Every Curious Mind | Para cada mente curiosa | Para mentes curiosas |
| Use the power of your voice to bring words to life | Usa el poder de tu voz para dar vida a las palabras | Dale vida a las palabras con tu voz |

### 6. Avoid jargon unless in an educational article

| Context | Approach |
|---|---|
| Homepage, marketing, CTAs | Don't use "decodificable". Say "libros que tu hijo puede leer solo" or "libros fáciles de leer" |
| Blog posts about reading science | "Decodificable" is fine — parents reading those articles expect technical terms |
| Book category descriptions | Avoid jargon. Keep it inviting |

### 7. Use infinitives for slogans, not gerunds

Spanish uses infinitives for taglines and calls to action. English overuses gerunds (-ing), and translators often carry that over.

| Don't write | Write instead |
|---|---|
| Aprendiendo a amar la lectura | Aprender a amar la lectura |
| Descubriendo nuevos mundos | Descubrir nuevos mundos |
| Mejorando la lectura de tu hijo | Mejorar la lectura de tu hijo |

---

## Specific Terms

| English | Spanish (LatAm) | Notes |
|---|---|---|
| Science of Reading | Ciencia de la Lectura | Keep capitalized as a proper noun |
| decodable books | libros decodificables | OK in educational articles; avoid in marketing |
| phonics | fonética | Universally understood |
| read-aloud | lectura en voz alta | |
| scope and sequence | guía de niveles | Or "orden de fonética" in casual contexts |
| reluctant readers | niños que no quieren leer | "Lectores reacios" sounds clinical |
| sight words | palabras de uso frecuente | |
| blends | combinaciones | |
| digraphs | dígrafos | |
| CVC words | palabras CVC | Keep the abbreviation |
| free trial | prueba gratis | |
| Mom's Choice Award | Premio Mom's Choice | Keep the English award name |

---

## Tone by Article Type

| Article Type | Tone |
|---|---|
| Blog posts (tips for parents) | Friendly, practical, conversational |
| Blog posts (reading science) | Accessible expert — like a teacher explaining to a parent |
| Blog posts (dyslexia) | Empathetic, supportive, never clinical |

---

## Translation Prompt

Use this prompt to generate the Spanish translation:

```
Translate the following blog post from English to Latin American Spanish for the Bookbot website.

RULES:
- Translate ALL frontmatter values: title, description, summary, heading, alt text, category names.
- Keep these fields UNCHANGED: slug, date, author slug, image (hero) path, layout, type, all YAML keys, markdown links/URLs, category (keep the English folder name).
- For inline illustration image paths: replace the filename with the `-es` variant (e.g., `/images/updates/phonics-diagram.png` → `/images/updates/phonics-diagram-es.png`). The hero image path stays unchanged (photographic, no text to translate).
- ADD a `url:` field to the front matter with the fully translated Spanish URL path. The URL structure is: `/es/articulos/[translated-category]/[translated-slug]/`. Use the category translations below and create a short, natural Spanish slug for the article (not a word-for-word translation of the English slug — write it the way a Spanish speaker would search for the topic).
- Translate the full article body.

STYLE (follow strictly):
- Write for Latin American parents. Tone: warm, direct, conversational — like a knowledgeable friend, not a textbook.
- Use "tu/tu hijo" (informal address), never "usted".
- Use active voice. Avoid passive constructions ("fue enviado" → "te enviamos").
- Keep sentences short. If a sentence has 3+ clauses, break it up.
- Use everyday vocabulary:
  - "maestros" not "educadores"
  - "ayuda" not "retroalimentación"
  - "dar" not "brindar" or "proporcionar"
  - "lectura" not "alfabetización" (unless specifically about literacy policy)
  - "app" not "aplicación"
  - "niños que no quieren leer" not "lectores reacios"
  - "animales salvajes" not "vida silvestre"
  - "premios" not "recompensas"
- For jargon: "decodable books" → "libros decodificables" is OK in educational articles. In marketing/casual contexts, say "libros que tu hijo puede leer solo".
- "Science of Reading" → "Ciencia de la Lectura" (capitalized, proper noun).
- Don't translate word-for-word from English. Rewrite ideas the way a Mexican or Colombian parent would naturally say them.
- Use infinitives for slogans/taglines, not gerunds ("Aprender a leer" not "Aprendiendo a leer").
- Spanish titles: only capitalize the first word and proper nouns.
- Don't start paragraphs with "Nos apasiona...", "Esta colección presenta...", or other corporate filler.
- Vary your phrasing — don't repeat the same construction in consecutive paragraphs.

OUTPUT: Return the complete translated .md file, ready to save.
```

Paste the full English article `.md` file content after this prompt and submit to the AI.

---

## File Paths

| | Path |
|---|---|
| English source | `content/en/updates/[category]/[slug].md` |
| Spanish destination | `content/es/updates/[category]/[slug].md` |

Keep the same filename (English slug) and category folder. Only translate the content inside the file. The file system structure mirrors the English source exactly — the translated URL path is handled by the `url:` front matter field, not by folder names.

### Spanish URL Path

Every Spanish article must have a `url:` field in its front matter with a fully translated path:

```
url: "/es/articulos/[translated-category]/[translated-slug]/"
```

For root-level articles (no category): `url: "/es/articulos/[translated-slug]/"`

### Category URL Translations

| English category folder | Spanish URL segment |
|---|---|
| `how-do-children-learn-to-read` | `como-aprenden-los-ninos-a-leer` |
| `dyslexia` | `dislexia` |
| `get-the-most-out-of-bookbot` | `aprovecha-bookbot-al-maximo` |

If a new category is created, translate the category name to a short, natural Spanish slug and add it to this table.

### Spanish Slug

The article slug in the `url:` field should be a natural Spanish translation of the topic — not a word-for-word translation of the English slug. Keep it short, lowercase, hyphenated, and readable. Examples:

| English slug | Spanish URL slug |
|---|---|
| `orthographic-mapping` | `mapeo-ortografico` |
| `paired-reading` | `lectura-en-pareja` |
| `emotional-stories-vocabulary` | `historias-emocionales-vocabulario` |
| `is-dyslexia-hereditary` | `la-dislexia-es-hereditaria` |

---

## Illustration Translation

Inline illustrations contain text labels that must be translated to Spanish. Photographic images (including the hero) have no text and are shared between languages.

For each inline illustration in the article:

1. **Read the English illustration** using the Read tool to identify all text labels
2. **Translate each label** to Latin American Spanish, applying the same vocabulary rules as the article text (e.g., "Reading Skills" → "Lectura", "Teachers" → "Maestros", "Feedback" → "Ayuda")
3. **Generate the Spanish illustration** using the "Illustration Translation" API call in `image-generation.md` — send the original image to Gemini with the label-by-label translation mapping
4. **Save** as `assets/updates/[descriptive-name]-es.png`
5. **Update the Spanish article** to reference the `-es` image paths:
   - English article: `![Alt text](/images/updates/phonics-diagram.png)`
   - Spanish article: `![Alt text traducido](/images/updates/phonics-diagram-es.png)`
6. **Translate the alt text** in the Spanish article as well

**What stays the same between languages:**
- Hero image (photographic, no text) — same path in both articles
- Any photographic inline image (rare) — same path if it contains no text

**What gets translated:**
- All illustration/infographic inline images that contain text labels
- Alt text for all images (even photographic ones with no visual text change)

**Cost:** ~$0.135 per illustration translated. Include in the running image generation cost total.

---

## Post-Translation Checklist

- [ ] Filename matches the English source (same slug)
- [ ] File is saved to `content/es/updates/` (matching the English subdirectory)
- [ ] `slug` field is unchanged (matches English slug)
- [ ] `url` field is set with fully translated Spanish path (`/es/articulos/[category]/[slug]/`)
- [ ] `category` field is unchanged (English folder name)
- [ ] `date` and `author` fields are unchanged
- [ ] Hero image path is unchanged (shared between languages)
- [ ] Inline illustration paths use `-es` suffix (e.g., `diagram-es.png`)
- [ ] Spanish illustrations generated for all text-containing illustrations
- [ ] Alt text translated for all images
- [ ] All Spanish text uses "tu" (not "usted")
- [ ] No passive voice in the first paragraph
- [ ] Title is not Capitalized Like English — first word and proper nouns only
- [ ] No "educadores", "retroalimentación", "brindar", or "lectores reacios"
- [ ] No "Nos apasiona..." or other corporate filler openers

---

## Common Mistakes to Watch For

1. **"Nos apasiona..."** — Corporate opener. Just say what you mean.
2. **"Esta colección presenta..."** — Sounds like a museum plaque. Try "Aquí encontrarás..."
3. **"A través de narrativas entretenidas y personajes memorables"** — Filler. Cut it.
4. **"Cada libro ayuda a los jóvenes lectores a..."** — Impersonal. Use "tu hijo".
5. **Long sentences with multiple clauses** — Break them up. Spanish reads better in shorter bursts for web content.
6. **Capitalizing Every Word In A Title** — Spanish only capitalizes the first word and proper nouns in titles.
7. **Passive constructions** — Flip them to active voice throughout.

---

## Usage in Blog Workflow

This skill is called from Phase 5.3 of `blog-writing-workflow.md`, immediately after the reviewer approves the English article (Step 5.2). The translation is included in the same publish commit as the English article.
