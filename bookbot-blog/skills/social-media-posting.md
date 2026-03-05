# Social Media Posting Skill

Post a published blog article to Facebook, Instagram, Pinterest, and LinkedIn with platform-tailored hooks designed for high click-through to the article.

---

## PURPOSE

After a blog article is published to the Bookbot website (Phase 5), this skill:

1. Waits for the Cloudflare Pages deployment to complete
2. Lets the user select an inline illustration for the social media posts
3. Generates platform-tailored hook text with hashtag strategies
4. Presents everything for user review and approval
5. Posts to all four platforms using their respective APIs
6. Reports the results with links to each published post

---

## CREDENTIALS CHECK

Before starting, verify all social media credentials are set:

```bash
echo "FACEBOOK_PAGE_TOKEN=${FACEBOOK_PAGE_TOKEN:+set}"
echo "FACEBOOK_PAGE_ID=${FACEBOOK_PAGE_ID:+set}"
echo "INSTAGRAM_ACCOUNT_ID=${INSTAGRAM_ACCOUNT_ID:+set}"
echo "PINTEREST_TOKEN=${PINTEREST_TOKEN:+set}"
echo "PINTEREST_BOARD_ID=${PINTEREST_BOARD_ID:+set}"
echo "LINKEDIN_TOKEN=${LINKEDIN_TOKEN:+set}"
echo "LINKEDIN_ORG_ID=${LINKEDIN_ORG_ID:+set}"
```

All 7 variables are required. If any are missing, stop and tell the user:

> Social media credentials are not fully configured. Please run `/setup` to add the missing variables.

Do NOT proceed with posting if any credential is missing.

---

## STEP 1: VERIFY ARTICLE IS PUBLISHED

Confirm that Phase 5.3 (publishing) completed successfully. From the workflow context, extract:

| Value | Source |
|-------|--------|
| `ARTICLE_URL` | Production URL from Step 5.4 (e.g., `https://www.bookbotkids.com/updates/how-do-children-learn-to-read/phonemic-awareness-activities/`) |
| `ARTICLE_TITLE` | From article front matter `title` field |
| `ARTICLE_HEADING` | From article front matter `heading` field |
| `ARTICLE_DESCRIPTION` | From article front matter `description` field |
| `ARTICLE_SLUG` | From article front matter `slug` field |
| `ARTICLE_BODY` | The full article text (for hook generation context) |
| `HERO_IMAGE_PATH` | Local path to the hero image (e.g., `assets/updates/[slug].png`) |
| `INLINE_IMAGE_PATHS` | List of all inline illustration image files in `assets/updates/` for this article |

If the article has not been published (Phase 5.3 did not complete), stop:

> Social media posting requires the article to be published first. Please complete the publishing step before running this phase.

---

## STEP 2: WAIT FOR DEPLOYMENT

The blog is hosted on Cloudflare Pages and auto-deploys when commits land on `main`. All social media posts will use the `bookbotkids.com` production URL for the image, so the deployment must complete before posting.

Poll the production article URL every 5 minutes until it returns HTTP 200:

```bash
ARTICLE_URL="https://www.bookbotkids.com/updates/[category]/[slug]/"
MAX_ATTEMPTS=12

for i in $(seq 1 $MAX_ATTEMPTS); do
  STATUS=$(curl -s -o /dev/null -w "%{http_code}" "$ARTICLE_URL")
  if [ "$STATUS" = "200" ]; then
    echo "Deployment confirmed: $ARTICLE_URL is live."
    break
  fi
  echo "Waiting for deployment... attempt $i/$MAX_ATTEMPTS (status: $STATUS)"
  sleep 300
done

if [ "$STATUS" != "200" ]; then
  echo "Article not yet live after $((MAX_ATTEMPTS * 5)) minutes."
fi
```

If the article URL does not return 200 after all attempts, stop and tell the user:

> The article does not appear to be live yet. Check the Cloudflare Pages deployment status and retry social media posting once the site has deployed.

Once the article URL returns 200, also verify the image URL resolves. Try the `.png` path first, then `.webp` (Hugo may convert):

```bash
IMAGE_NAME="[selected-image-name]"
IMAGE_URL=""

# Try PNG first
STATUS=$(curl -s -o /dev/null -w "%{http_code}" "https://www.bookbotkids.com/images/updates/${IMAGE_NAME}.png")
if [ "$STATUS" = "200" ]; then
  IMAGE_URL="https://www.bookbotkids.com/images/updates/${IMAGE_NAME}.png"
fi

# Try WebP if PNG didn't resolve
if [ -z "$IMAGE_URL" ]; then
  STATUS=$(curl -s -o /dev/null -w "%{http_code}" "https://www.bookbotkids.com/images/updates/${IMAGE_NAME}.webp")
  if [ "$STATUS" = "200" ]; then
    IMAGE_URL="https://www.bookbotkids.com/images/updates/${IMAGE_NAME}.webp"
  fi
fi

if [ -z "$IMAGE_URL" ]; then
  echo "ERROR: Image not accessible at bookbotkids.com"
fi
```

Store the verified `IMAGE_URL` for use in all platform posts. Do not proceed if the image URL does not resolve.

---

## STEP 3: SELECT SOCIAL MEDIA IMAGE

Present all inline illustrations (not the hero image) to the user for selection. The hero image is photographic and typically 16:9, while inline illustrations use the Bookbot flat infographic style which is more eye-catching on social feeds.

1. List the inline images generated for this article from `assets/updates/` (excluding the hero image `[slug].png`)
2. Use the Read tool to display each illustration to the user
3. Ask:

> **Which illustration would you like to use for the social media posts?**
>
> This image will be used across all four platforms (Facebook, Instagram, Pinterest, LinkedIn).
>
> [1] [descriptive-name-1].png
> [2] [descriptive-name-2].png
> [3] [descriptive-name-3].png
> ...

4. Store the user's selection. The image URL verification in Step 2 runs after this selection is made (or re-runs if not already verified for this specific image).

---

## STEP 4: GENERATE PLATFORM HOOKS

Generate tailored hook text for each platform based on the full article content. The hooks should create curiosity or urgency to click through to the article.

**All hooks must:**
- Follow Bookbot voice (warm, evidence-based, not salesy) — see `voice-guide.md`
- Use specific stats, findings, or surprising data from the article
- Avoid all AI phrases listed in `voice-guide.md` ("diving into", "here's what surprised me when I started", "let's dive in", "in today's fast-paced world")
- Include the article URL where the platform supports clickable links
- Never include CTAs to download Bookbot or sales language

---

### 4a. Facebook Hook

**Format:** 2–3 sentences + article link + 1–3 hashtags

**Tone:** Conversational, warm, question-based or surprising-stat lead. Write as if sharing an interesting finding with a friend who's a parent.

**Structure:**
1. Opening question or surprising stat from the article
2. Brief tease of what the article covers (create curiosity gap)
3. Article link on its own line
4. 1–3 targeted hashtags on a new line (community/topic-focused, not generic)

**Hashtag strategy (1–3):**
- Keep minimal — Facebook hashtags have low discoverability
- Use topic-specific or community tags only
- Examples: #ReadingScience, #EarlyLiteracy, #PhonicsForKids, #DyslexiaAwareness, #KidsReading, #ScienceOfReading

**Example:**

```
Did you know that children who receive just 15 minutes of daily phonics practice read 1.5 standard deviations ahead of their peers? That's a massive difference, and it starts earlier than most parents think.

https://www.bookbotkids.com/updates/how-do-children-learn-to-read/phonemic-awareness-activities/

#ReadingScience #EarlyLiteracy
```

---

### 4b. Instagram Hook

**Format:** Bold opening line + 3–5 short lines with line breaks + "Link in bio" CTA + 3–11 hashtags

**Tone:** Attention-grabbing first line (only ~125 characters show before "...more" truncation). Slightly more casual than the blog, but still evidence-based.

**Structure:**
1. Bold opening line — a surprising stat, provocative question, or counterintuitive finding (this is what shows in the feed before truncation)
2. Blank line
3. 2–4 short lines expanding on the key takeaway, separated by line breaks
4. Blank line
5. "Tap the link in our bio for the full article." (links in Instagram captions are NOT clickable)
6. Blank line
7. 3–11 targeted hashtags

**Hashtag strategy (3–11):**

Select from these categories based on article topic relevance. Prioritise niche tags over generic ones. Mix high-volume discovery tags with specific ones to balance Explore page reach and targeted engagement.

| Category | Examples | Notes |
|----------|----------|-------|
| **Brand** | #Bookbot, #BookbotApp, #BookbotKids | Include 1–2 per post |
| **High-volume discovery** | #KidsReading, #ParentingTips, #EarlyLearning, #ReadingIsFun, #LiteracyMatters, #LearnToRead | Reach the Explore page. Pick 2–4 relevant ones. |
| **Niche/topic-specific** | Derived from article topic. E.g., #PhonicsActivities, #DyslexiaSupport, #ReadingResearch, #ScienceOfReading, #PhonemicAwareness, #ReadingIntervention, #DecodableBooks | Targeted reach. Pick 2–4 relevant ones. |
| **Audience** | #MumsOfInstagram, #TeachersOfInstagram, #HomeschoolMum, #PrimaryTeacher, #KinderTeacher, #ParentsOfReaders | Pick 1–2 relevant ones. |

**Line breaks:** The caption must contain actual newlines (not literal `\n` text). When posting via the API, write the caption to a temp file with real line breaks — see Step 7a.

**Example:**

```
Children who practise phonics for just 15 minutes a day read significantly ahead of their peers.

Not hours. Not expensive tutoring. Just 15 focused minutes.

New research shows that consistent, short practice sessions outperform longer, less frequent ones for early readers.

Tap the link in our bio for the full article.

#Bookbot #ScienceOfReading #PhonicsActivities #KidsReading #EarlyLiteracy #ParentingTips #TeachersOfInstagram
```

---

### 4c. Pinterest Hook

**Format:** Title (max 100 chars) + Description (max 500 chars) + destination link

**Tone:** Keyword-rich for search discovery. Pinterest is a visual search engine — the description should be packed with terms parents and teachers would search for.

**No hashtags** — Pinterest's search algorithm prioritises keyword-rich descriptions over hashtags.

**Structure:**
- **Pin Title:** Clear, keyword-rich, benefit-focused. Mirror the article's primary keyword and value proposition. Max 100 characters.
- **Pin Description:** 2–3 sentences explaining what the reader will learn. Pack with search-relevant terms (reading activities, phonics practice, early literacy, learn to read, etc.). The article URL is set as the pin's destination link (separate from description).

**Example:**

- **Title:** "15-Minute Phonics Activities That Help Kids Read Ahead"
- **Description:** "Evidence-based phonemic awareness activities that help children ages 5–9 become stronger readers. Learn why daily phonics practice is the single most effective strategy for early reading success, plus practical activities you can use at home or in the classroom today."
- **Link:** `https://www.bookbotkids.com/updates/.../`

---

### 4d. LinkedIn Hook

**Format:** 2–3 short paragraphs + article link + 3–5 hashtags

**Tone:** Professional but warm, thought-leadership angle. Write as a researcher or education professional sharing findings relevant to literacy, edtech, or child development.

**Structure:**
1. Opening insight or question relevant to educators, parents, or edtech professionals
2. Key research finding from the article with specific data
3. Connection to Bookbot's mission or what the team is working on (informational, not sales)
4. Article link on its own line
5. 3–5 professional, industry-specific hashtags on a new line

**Hashtag strategy (3–5):**

| Category | Examples | Notes |
|----------|----------|-------|
| **Industry** | #EdTech, #LiteracyEducation, #ScienceOfReading, #EarlyChildhoodEducation | Pick 1–2 |
| **Professional** | #ReadingResearch, #EducationTechnology, #ChildDevelopment, #LearningToRead, #ReadingIntervention | Pick 1–2 |
| **Broader reach** | #Innovation, #AI, #Education, #Impact, #EvidenceBased | Pick 1 |

Keep professional and relevant to the article topic. Never use casual or consumer-facing tags.

**Example:**

```
New research shows that just 15 minutes of daily phonics practice produces reading gains of 1.5 standard deviations — that's the difference between a struggling reader and a confident one.

At Bookbot, we analyse real reading data from thousands of children to understand exactly when and how these interventions work best. The evidence is clear: consistency matters more than duration.

We break down the research and share practical strategies for parents and teachers:
https://www.bookbotkids.com/updates/how-do-children-learn-to-read/phonemic-awareness-activities/

#ScienceOfReading #EdTech #ReadingResearch #EarlyChildhoodEducation #LiteracyEducation
```

---

## STEP 5: REVIEW AND APPROVAL

Present the selected image and all four platform hooks for user review.

**Display format:**

```
## Social Media Posts for Review

**Selected Image:** [descriptive-name].png
[Show the image using the Read tool]

**Article:** [ARTICLE_TITLE]
**URL:** [ARTICLE_URL]
**Image URL:** [IMAGE_URL]

---

### Facebook

[Generated Facebook hook text with hashtags]

---

### Instagram

[Generated Instagram hook text with hashtags]

---

### Pinterest

**Title:** [Pin title]
**Description:** [Pin description]
**Link:** [Article URL]

---

### LinkedIn

[Generated LinkedIn hook text with hashtags]

---

Would you like to approve these posts, or adjust any of the hooks?
You can also change the selected image.
```

Wait for explicit approval before posting. If the user requests changes:

1. Revise the specific hook(s) as requested
2. If the user wants a different image, go back to Step 3
3. Re-present the updated version
4. Repeat until the user gives explicit approval (e.g., "looks good", "approve", "post it", "go ahead")

---

## STEP 6: POST TO FACEBOOK

Post a photo with the hook text to the Bookbot Facebook Page.

**Important:** Always write the hook text to a temp file first. Do not pass the message inline in the curl command — special characters, newlines, and quotes will break the command. Always use the approved hook for **this** article (not a previous one).

```bash
# Write the approved Facebook hook text to a temp file
cat > /tmp/fb-hook.txt << 'HOOKEOF'
[Facebook hook text for THIS article — include article link and hashtags]
HOOKEOF

curl -s -X POST \
  "https://graph.facebook.com/v22.0/$FACEBOOK_PAGE_ID/photos" \
  -F "url=$IMAGE_URL" \
  -F "message=</tmp/fb-hook.txt" \
  -F "access_token=$FACEBOOK_PAGE_TOKEN"
```

**Response parsing:**

```json
{"id": "PHOTO_ID", "post_id": "PAGE_ID_POST_ID"}
```

Extract the `post_id`. The Facebook post URL is:
```
https://www.facebook.com/$FACEBOOK_PAGE_ID/posts/[POST_ID_SUFFIX]
```

Where `[POST_ID_SUFFIX]` is the part after the underscore in `post_id` (e.g., if `post_id` is `123_456`, the URL uses `456`).

**Error handling:**
- `(#200) Requires extended permission`: Token is missing required permissions. Tell the user to re-run `/setup` and check Facebook app permissions.
- `(#100) Invalid parameter`: Check image URL accessibility or message formatting.
- `(#4) Application request limit reached`: Rate limit hit (unlikely for single posts). Report and continue to next platform.
- Any other error: Show the full error response and continue to next platform.

**Cleanup:** `rm -f /tmp/fb-hook.txt`

---

## STEP 7: POST TO INSTAGRAM

Post a photo with caption to the Bookbot Instagram account. Instagram uses a two-step process through the Facebook Graph API.

### Step 7a: Create Media Container

**Important:** Write the caption to a temp file with actual newlines — do NOT use literal `\n` text in the string, as the API will display those as visible characters. Always use the approved hook for **this** article.

```bash
# Write the approved Instagram caption to a temp file with real line breaks
cat > /tmp/ig-caption.txt << 'CAPTIONEOF'
[Instagram hook text for THIS article — with real blank lines between sections, hashtags at the end]
CAPTIONEOF

curl -s -X POST \
  "https://graph.facebook.com/v22.0/$INSTAGRAM_ACCOUNT_ID/media" \
  --data-urlencode "image_url=$IMAGE_URL" \
  --data-urlencode "caption@/tmp/ig-caption.txt" \
  -d "access_token=$FACEBOOK_PAGE_TOKEN"
```

The `caption@/tmp/ig-caption.txt` syntax tells curl to read the caption value from the file and URL-encode it, preserving actual newlines. Hashtags go directly in the caption text.

**Response:**

```json
{"id": "CREATION_ID"}
```

Extract the `id` as `CREATION_ID`.

If the response contains an error instead of an `id`, check:
- `(#36003)` or image-related errors: The image URL is not publicly accessible. Verify the deployment completed and the URL returns 200.
- `(#25)` or caption errors: Caption exceeds 2,200 characters. Trim hashtags to fit.

### Step 7b: Wait for Container Processing

```bash
sleep 10
```

For photos, the container is typically ready within a few seconds. A 10-second wait is sufficient.

### Step 7c: Publish the Container

```bash
curl -s -X POST \
  "https://graph.facebook.com/v22.0/$INSTAGRAM_ACCOUNT_ID/media_publish" \
  -d "creation_id=$CREATION_ID" \
  -d "access_token=$FACEBOOK_PAGE_TOKEN"
```

**Response:**

```json
{"id": "MEDIA_ID"}
```

The `MEDIA_ID` is the Instagram post ID. Instagram does not return a direct URL in the API response. Note the Media ID in the summary report — the user can find the post on their Instagram page.

**Error handling:**
- If container creation fails, report the error and skip Instagram.
- If publishing fails (e.g., container not ready), wait an additional 10 seconds and retry once. If it still fails, report and continue.

**Cleanup:** `rm -f /tmp/ig-caption.txt`

---

## STEP 8: POST TO PINTEREST

Create a pin on the Bookbot Pinterest board with the illustration, title, description, and a link to the article.

```bash
curl -s -X POST \
  "https://api.pinterest.com/v5/pins" \
  -H "Authorization: Bearer $PINTEREST_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "board_id": "'"$PINTEREST_BOARD_ID"'",
    "title": "PIN_TITLE",
    "description": "PIN_DESCRIPTION",
    "link": "'"$ARTICLE_URL"'",
    "alt_text": "ALT_TEXT_FROM_ARTICLE",
    "media_source": {
      "source_type": "image_url",
      "url": "'"$IMAGE_URL"'"
    }
  }'
```

Replace `PIN_TITLE`, `PIN_DESCRIPTION`, and `ALT_TEXT_FROM_ARTICLE` with the approved Pinterest hook content and the alt text from the selected illustration's markdown reference. Use a temp file for the JSON payload if needed to avoid shell escaping issues:

```bash
cat > /tmp/pinterest-pin.json << PINEOF
{
  "board_id": "$PINTEREST_BOARD_ID",
  "title": "PIN_TITLE",
  "description": "PIN_DESCRIPTION",
  "link": "$ARTICLE_URL",
  "alt_text": "ALT_TEXT",
  "media_source": {
    "source_type": "image_url",
    "url": "$IMAGE_URL"
  }
}
PINEOF

curl -s -X POST \
  "https://api.pinterest.com/v5/pins" \
  -H "Authorization: Bearer $PINTEREST_TOKEN" \
  -H "Content-Type: application/json" \
  -d @/tmp/pinterest-pin.json
```

**Response:**

```json
{"id": "PIN_ID", ...}
```

The pin URL is: `https://www.pinterest.com/pin/PIN_ID/`

**Error handling:**
- `403`: Token lacks required scope. Need `pins:write`. Direct user to `/setup`.
- `429`: Rate limit (1,000 writes/hour — extremely unlikely). Report and continue.
- Any other error: Show full error response and continue.

**Cleanup:** `rm -f /tmp/pinterest-pin.json`

---

## STEP 9: POST TO LINKEDIN

Post an article with image to the Bookbot LinkedIn company page. LinkedIn requires a three-step process: register the image upload, upload the binary, then create the post.

### Step 9a: Register Image Upload

```bash
curl -s -X POST \
  "https://api.linkedin.com/rest/images?action=initializeUpload" \
  -H "Authorization: Bearer $LINKEDIN_TOKEN" \
  -H "LinkedIn-Version: 202501" \
  -H "X-Restli-Protocol-Version: 2.0.0" \
  -H "Content-Type: application/json" \
  -d '{
    "initializeUploadRequest": {
      "owner": "urn:li:organization:'"$LINKEDIN_ORG_ID"'"
    }
  }'
```

**Response:**

```json
{
  "value": {
    "uploadUrl": "https://www.linkedin.com/dms-uploads/...",
    "image": "urn:li:image:..."
  }
}
```

Extract `uploadUrl` and `image` (the image URN) from the response.

### Step 9b: Upload the Image

Download the image from the production URL, then upload the binary to LinkedIn's upload endpoint:

```bash
# Download the image to a temp file
curl -s -o /tmp/linkedin-image.png "$IMAGE_URL"

# Upload to LinkedIn
curl -s -X PUT \
  "$UPLOAD_URL" \
  -H "Authorization: Bearer $LINKEDIN_TOKEN" \
  --upload-file /tmp/linkedin-image.png
```

### Step 9c: Create the Post

```bash
cat > /tmp/linkedin-post.json << LIEOF
{
  "author": "urn:li:organization:$LINKEDIN_ORG_ID",
  "commentary": "LINKEDIN_HOOK_TEXT",
  "visibility": "PUBLIC",
  "distribution": {
    "feedDistribution": "MAIN_FEED",
    "targetEntities": [],
    "thirdPartyDistributionChannels": []
  },
  "content": {
    "article": {
      "source": "$ARTICLE_URL",
      "title": "$ARTICLE_TITLE",
      "description": "$ARTICLE_DESCRIPTION",
      "thumbnail": "$IMAGE_URN"
    }
  },
  "lifecycleState": "PUBLISHED",
  "isReshareDisabledByAuthor": false
}
LIEOF

curl -s -i -X POST \
  "https://api.linkedin.com/rest/posts" \
  -H "Authorization: Bearer $LINKEDIN_TOKEN" \
  -H "LinkedIn-Version: 202501" \
  -H "X-Restli-Protocol-Version: 2.0.0" \
  -H "Content-Type: application/json" \
  -d @/tmp/linkedin-post.json
```

Replace `LINKEDIN_HOOK_TEXT` with the approved LinkedIn hook text (including article link and hashtags), `$ARTICLE_TITLE` with the article title, `$ARTICLE_DESCRIPTION` with the meta description, and `$IMAGE_URN` with the URN from Step 9a.

**Response:** The post ID is returned in the `x-restli-id` response header (not the body). Use `-i` flag to capture headers:

```
x-restli-id: urn:li:share:7123456789012345678
```

The LinkedIn post URL is:
```
https://www.linkedin.com/feed/update/urn:li:share:POST_ID/
```

**Error handling:**
- `401`: Token expired. LinkedIn tokens expire after 60 days. Tell the user to re-run `/setup` to refresh the token.
- `403`: Missing `w_organization_social` permission. Direct user to `/setup`.
- `422`: Invalid payload. Show the full error for debugging.
- If any step fails, report the error and continue.

**Cleanup:** `rm -f /tmp/linkedin-image.png /tmp/linkedin-post.json`

---

## STEP 10: SUMMARY REPORT

After posting to all platforms, present a summary:

```
## Social Media Posting Summary

**Article:** [ARTICLE_TITLE]
**URL:** [ARTICLE_URL]
**Image:** [descriptive-name].png

| Platform | Status | Link |
|----------|--------|------|
| Facebook | Posted | [View post](https://www.facebook.com/...) |
| Instagram | Posted | Media ID: [id] (visible on your Instagram page) |
| Pinterest | Posted | [View pin](https://www.pinterest.com/pin/.../) |
| LinkedIn | Posted | [View post](https://www.linkedin.com/feed/update/.../) |

**Cost:** $0.00 (all platforms use free API tiers)
```

If any platform failed, show the error:

```
| Instagram | FAILED | Error: Image not accessible at public URL. Check deployment status. |
```

---

## RATE LIMITS & COST

All social media APIs are free to use for posting to your own pages and accounts. No per-post charges.

| Platform | Rate Limit | Token Expiry |
|----------|-----------|--------------|
| Facebook | 200 calls/hour per Page | Long-lived Page tokens do not expire (revocable) |
| Instagram | 25 posts per 24 hours | Uses Facebook Page token (same expiry) |
| Pinterest | 1,000 writes per hour | Tokens valid for 30 days (refresh available) |
| LinkedIn | 100 posts per day per organisation | Tokens expire after 60 days |

**Cost per run:** $0.00

Since each blog article generates exactly 1 post per platform, rate limits will never be an issue in normal use.

---

## USAGE IN BLOG WORKFLOW

This skill is called from Phase 6 of `blog-writing-workflow.md`, immediately after publication is confirmed in Step 5.4. The workflow passes forward:

- Article title, slug, description, and body text
- Production URL
- Inline image file paths (from Phase 3.5)
- Hero image path

The social media posting skill handles everything from there: image selection, hook generation, user review, and posting to all four platforms.
