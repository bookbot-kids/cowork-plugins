# Keyword Research Skill — DataForSEO Integration

Use this skill to find secondary keywords from a main keyword using the DataForSEO Google Ads API.

---

## PURPOSE

Given a main keyword (e.g., "phonemic awareness"), this skill:

1. Calls the DataForSEO API to retrieve related keywords with real search volume data
2. Filters and scores candidates for blog SEO relevance
3. Returns a curated set of 8–12 keywords (3–4 primary, 4–6 long-tail, 1–2 supporting)

---

## CREDENTIALS CHECK

Before calling the API, verify the auth token is set:

```bash
echo "${DATAFORSEO_AUTH:+set}"
```

If missing, tell the user:

> DataForSEO credentials are not configured. Please run `/setup` to add your `DATAFORSEO_AUTH` token.

Do NOT proceed with the API call if credentials are missing. Fall back to AI-generated keyword suggestions instead (see "Fallback" below).

---

## STEP 1: CALL THE API

Use the pre-encoded `$DATAFORSEO_AUTH` token directly in the Authorization header:

```bash
curl -s -X POST \
  -H "Authorization: Basic $DATAFORSEO_AUTH" \
  -H "Content-Type: application/json" \
  -d "[{\"keywords\":[\"MAIN_KEYWORD\"], \"sort_by\":\"search_volume\", \"include_adult_keywords\":false}]" \
  "https://api.dataforseo.com/v3/keywords_data/google_ads/keywords_for_keywords/live"
```

Replace `MAIN_KEYWORD` with the actual keyword. Wrap it in single quotes or escape spaces if needed.

**Error handling:**
- If `status_code` in the response is not `20000`, show the `status_message` to the user and stop
- If the result array is empty, report that no related keywords were found and proceed to Fallback

---

## STEP 2: PARSE AND FILTER RESULTS

The response contains a `result` array. Each item has:

| Field | Use |
|-------|-----|
| `keyword` | The related keyword string |
| `search_volume` | Average monthly searches (primary ranking signal) |
| `competition` | `LOW`, `MEDIUM`, or `HIGH` (Google Ads competition) |
| `competition_index` | 0–100 numeric version of competition |
| `cpc` | Cost per click in USD |
| `keyword_annotations.concepts` | Semantic concept groups (relevance signal) |

**Filter out candidates that:**
- Have `search_volume` of 0 or null
- Are exact duplicates of the main keyword
- Are clearly off-topic (use concept groups to judge — brand names unrelated to the topic, foreign-language variants if targeting English speakers)
- Have `competition_index` of 100 with very low `search_volume` (< 500) — not worth targeting

**Keep the remaining candidates and score them:**

```
score = search_volume × relevance_bonus × competition_penalty

relevance_bonus:
  - Keyword shares concept group with main keyword → 1.5×
  - Keyword is a phrase variant of main keyword → 1.3×
  - Keyword is topically adjacent → 1.0×
  - Keyword is only loosely related → 0.7×

competition_penalty:
  - LOW → 1.2× (easier to rank)
  - MEDIUM → 1.0×
  - HIGH → 0.8×
```

Sort candidates by score descending.

---

## STEP 3: SELECT KEYWORDS

From the scored candidates, select:

- **1 primary keyword** — the highest-scoring, most relevant term. This becomes the `keyword:` field. Should be 2–4 words, directly about the article topic, and have meaningful search volume.
- **4–8 secondary keywords** — remaining relevant candidates sorted by score. These become the `secondaryKeywords:` list. Include long-tail variants, related terms, and topically adjacent phrases. Exclude brand names and off-topic results.

---

## STEP 4: PRESENT RESULTS

Format the output as follows:

```
## Keyword Research Results for: "[main keyword]"

**Data source:** DataForSEO Google Ads API
**Results analysed:** [N] keywords
**Date:** [today's date]

---

### Suggested Front Matter

keyword: [primary keyword]
secondaryKeywords:
  - [secondary keyword 1]
  - [secondary keyword 2]
  - [secondary keyword 3]
  - [secondary keyword 4]
  - [secondary keyword 5]

---

### Search Volume Reference

| Role | Keyword | Monthly Searches | Competition |
|------|---------|-----------------|-------------|
| primary | [keyword] | [volume] | [LOW/MEDIUM/HIGH] |
| secondary | [keyword] | [volume] | [LOW/MEDIUM/HIGH] |
...

---

**Recommended focus:** [1–2 sentence explanation of the primary keyword choice and any standout secondary keywords worth emphasising in the article body.]
```

After presenting, ask:

> Would you like to use these keywords, adjust the list, or add your own? Once approved, I'll carry them through into the article.

Wait for the user to confirm or adjust before proceeding.

---

## FALLBACK: AI-GENERATED SUGGESTIONS

If the DataForSEO API is unavailable or unconfigured, generate keyword suggestions using internal knowledge:

1. Analyze the main keyword and article topic
2. Generate 8–12 keyword suggestions following the same 3-category structure
3. Note the estimated search intent (not real volume data) for each
4. Present with a disclaimer:

> **Note:** These are AI-generated suggestions without real search volume data. Configure DataForSEO credentials via `/setup` to get accurate search data.

---

## USAGE IN BLOG WORKFLOW

This skill is called from Phase 0 of `blog-writing-workflow.md`. Pass the article topic as the main keyword. If the topic is a long sentence or URL, extract the core 2–3 word phrase first (e.g., "phonemic awareness activities for kids" from an article about phonemic awareness interventions).

The selected keywords feed into Phase 3 SEO Integration.
