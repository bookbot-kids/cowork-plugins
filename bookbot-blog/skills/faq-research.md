# FAQ Research Skill — DataForSEO SERP Integration

Use this skill to extract "People Also Ask" questions from Google search results for a given keyword. These become the basis for an FAQ section in the article.

---

## PURPOSE

Given a primary keyword, this skill:

1. Calls the DataForSEO SERP API to retrieve the live Google search results page
2. Extracts all `people_also_ask` questions and their answers
3. Filters for relevance to the article topic
4. Returns a curated set of 4–6 FAQ questions with answers for use in the article

---

## CREDENTIALS CHECK

Before calling the API, verify the auth token is set:

```bash
echo "${DATAFORSEO_AUTH:+set}"
```

If missing, stop and fall back to AI-generated FAQs (see "Fallback" below).

---

## STEP 1: CALL THE SERP API

Use the pre-encoded `$DATAFORSEO_AUTH` token directly in the Authorization header:

```bash
curl -s -X POST \
  -H "Authorization: Basic $DATAFORSEO_AUTH" \
  -H "Content-Type: application/json" \
  -d '[{"keyword":"MAIN_KEYWORD", "location_code":2036, "language_code":"en", "device":"desktop", "os":"macos", "depth":10}]' \
  "https://api.dataforseo.com/v3/serp/google/organic/live/advanced"
```

Replace `MAIN_KEYWORD` with the primary keyword. Use `location_code: 2036` (Australia) by default, since Bookbot's primary audience is Australian parents and teachers.

**Cost note:** Each SERP call costs $0.002. Report this at the end alongside keyword research costs.

**Error handling:**
- If `status_code` is not `20000`, show the error message and fall back to AI-generated FAQs
- If `result` array is empty, report no results and fall back

---

## STEP 2: EXTRACT PEOPLE ALSO ASK QUESTIONS

From the response, navigate to `tasks[0].result[0].items` and find all items where `type == "people_also_ask"`. Each such item contains an `items` array of individual questions.

For each question, extract:

| Field | Location in response |
|-------|---------------------|
| Question text | `items[].title` |
| Answer snippet | `items[].expanded_element[0].description` |
| Source URL | `items[].expanded_element[0].url` |
| Source domain | `items[].expanded_element[0].domain` |

If `expanded_element` contains type `people_also_ask_ai_overview_expanded_element`, use the `text` field instead of `description`.

---

## STEP 3: FILTER AND SELECT QUESTIONS

From the extracted questions, select 4–6 that are:

**Keep if:**
- Directly relevant to the article topic and Bookbot's audience (parents, teachers of children 5–9)
- Answerable from the article's content or from reading science knowledge
- Question intent is informational (not navigational/commercial — skip "Where can I buy...")
- Written as a genuine parent/teacher question

**Discard if:**
- Off-topic (e.g., questions about unrelated books, authors, or publishing industry)
- Already fully answered in the main article body (redundant)
- Duplicate intent to another selected question
- Primarily about brands, prices, or where-to-buy

**Prefer questions that:**
- A parent or teacher would realistically ask
- Have a clear, concise answer (can be answered in 2–4 sentences)
- Cover different angles of the topic (don't pick 4 questions about the same sub-topic)

---

## STEP 4: RESEARCH AND WRITE FAQ ANSWERS

For each selected question, write a well-researched answer. The PAA snippet from the SERP response is a signal for what question to answer, not a source to rewrite. Research each answer properly:

1. **Use the article's evidence bank** (from Phase 2) where the article topic directly answers the question — cite specific findings or studies already gathered
2. **Run a targeted web search** for questions that fall outside the article's core evidence (e.g., "What does it mean for a book to be decodable?" if the article is about phonics more broadly) — prioritise PMC, university sources, and reading science organisations
3. **Write from knowledge** for questions where the answer is well-established in reading science and doesn't require a specific citation

**Answer quality standards:**
- 2–4 sentences — concise and scannable
- Plain language (Grade 8–10 reading level)
- No jargon without immediate translation
- Informational tone — not salesy
- Include a markdown link to an internal Bookbot article where one exists and is genuinely relevant (max 1–2 internal links across the whole FAQ block)
- Include an external citation link where a specific study supports the answer
- Reference Bookbot where genuinely relevant (max 1 mention across the whole FAQ block)
- Do NOT copy any PAA snippet verbatim

---

## STEP 5: PRESENT RESULTS

Format the output as follows:

```
## FAQ Research Results for: "[keyword]"

**Data source:** DataForSEO SERP API (Google Australia)
**People Also Ask questions found:** [N]
**Selected for article:** [N]
**API cost:** $0.002

---

### Suggested Front Matter (faqs field)

faqs:
  - question: "[Question 1]"
    answer: "[2–4 sentence answer in Bookbot voice. Markdown links allowed.]"
  - question: "[Question 2]"
    answer: "[2–4 sentence answer in Bookbot voice. Markdown links allowed.]"
  - question: "[Question 3]"
    answer: "[2–4 sentence answer in Bookbot voice. Markdown links allowed.]"
  - question: "[Question 4]"
    answer: "[2–4 sentence answer in Bookbot voice. Markdown links allowed.]"

---

**Discarded questions:** [list any PAA questions that were filtered out, with brief reason]
```

After presenting, ask:

> Would you like to use these FAQs, adjust any answers, swap questions, or add your own? Once approved, I'll carry them into the article front matter.

Wait for the user to confirm or adjust before proceeding.

---

## FRONT MATTER PLACEMENT

Once approved, the FAQs go into the article's YAML front matter as the `faqs` field (see `hugo-output-spec.md`). The Hugo template renders them automatically below the article body — do NOT add a FAQ section to the article body markdown.

---

## FALLBACK: AI-GENERATED FAQS

If the DataForSEO API is unavailable or unconfigured, generate FAQ questions using internal knowledge:

1. Think of 4–6 questions a parent or teacher would realistically search after reading the article
2. Write Bookbot-voice answers for each
3. Present in the same `faqs:` front matter format as the API path
4. Note the source:

> **Note:** These are AI-generated FAQs. Configure DataForSEO credentials via `/setup` to pull real "People Also Ask" questions from Google.

---

## USAGE IN BLOG WORKFLOW

This skill is called automatically from Phase 0 (Step 0.2) of `blog-writing-workflow.md`, using the primary keyword approved in Step 0.1. It always runs — it is not optional. The approved `faqs` block goes directly into the article's YAML front matter.
