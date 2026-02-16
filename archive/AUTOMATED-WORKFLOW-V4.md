# AUTOMATED BLOG POST WORKFLOW - VERSION 4
## Fully Automated System with Quality Gates and Self-Correction

**Purpose:** Complete automation from topic input to publication-ready article with automated testing and self-correction.

**What This Workflow Does:**
- Takes minimal input (topic/URL + optional keywords)
- Automatically conducts research, builds evidence bank, generates article
- Runs automated quality tests (like unit tests)
- Self-corrects if tests fail
- Outputs publication-ready article + process documentation

**How to Use:**
1. Provide INPUT (topic/URL + optional keywords)
2. Run this entire workflow
3. System outputs article + documentation when all tests pass

---

## INPUT REQUIREMENTS

Provide the following:

### Required Input:
**TOPIC**: One of the following:
- General topic (e.g., "supporting struggling readers")
- Article URL to translate for parent audience
- Research paper URL/DOI to translate

### Optional Input:
**SEO KEYWORDS**: 1-12 keywords (can skip if you want natural flow without forced optimization)
- Primary keywords (3-4): Main search terms
- Long-tail keywords (4-6): Specific phrases like "my 2nd grader is struggling with reading"
- Supporting keywords (1-2): Related terms

**Note:** If keywords not provided, AI will ask if you want AI-generated keyword suggestions.

---

## AUTOMATED WORKFLOW EXECUTION

### PHASE 0: KEYWORD OPTIMIZATION CHECK (If keywords not provided)

**If writer did not provide SEO keywords:**

Ask: "Would you like me to suggest SEO keywords based on the research topic? I can analyze the research and suggest 8-12 relevant keywords for natural integration."

**Options:**
1. **Yes, suggest keywords** → AI analyzes research topic/URL, generates keyword suggestions, presents them to writer for approval
2. **No, skip keywords** → AI writes naturally without keyword optimization

**If "Yes, suggest keywords":**
- Analyze research topic/title
- Generate 8-12 keyword suggestions:
  - Primary keywords (3-4): Main search terms
  - Long-tail keywords (4-6): Specific parent/teacher search phrases
  - Supporting keywords (1-2): Related terms
- Present to writer: "Here are suggested SEO keywords: [list]. Approve or modify?"
- Writer approves → Use these keywords
- Writer modifies → Use modified keywords

**If "No, skip keywords":**
- Proceed with natural topic-relevant language only

---

### PHASE 1: AUTOMATED RESEARCH GATHERING

**Step 1.1: Conduct Web Searches (Automated)**
Execute searches automatically:
```
- "[topic] research 2024 2025 peer-reviewed"
- "[topic] meta-analysis cognitive neuroscience"
- "[topic] intervention effectiveness studies"
```

**Prioritize sources:**
- PMC (PubMed Central) articles
- Frontiers journals (open access)
- National Reading Panel reports
- University research (.edu domains)
- Meta-analyses and systematic reviews

**Minimum requirement:** 4-5 credible sources

**Step 1.2: Fetch and Read Papers (Automated)**
For each source found, automatically extract:
- Title, authors, year, journal
- Sample size (n=?)
- Specific findings (percentages, effect sizes, outcomes)
- Methodology (to assess credibility)
- Key quotes

**Step 1.3: Quality Filter (Automated)**
Automatically reject sources if:
- Can't access full paper to verify claims
- Sample size too small (n<50 for quantitative studies)
- Publication date pre-2000 (unless landmark study)
- Not peer-reviewed (blogs, news articles)

Automatically prefer sources with:
- Large sample sizes (n>100)
- Published within 10 years
- Meta-analyses or systematic reviews
- Reputable journals/institutions

---

### PHASE 2: AUTOMATED EVIDENCE ORGANIZATION

**Step 2.1: Build Evidence Bank (Automated)**
Automatically create structured evidence bank:

```markdown
## EVIDENCE BANK

### FINDING 1: [Brief description]
- **CLAIM**: What will be said in article
- **SOURCE**: Author(s), Year, Journal
- **DATA**: Exact statistic/finding with sample size
- **CITATION**: In-text citation format
- **URL**: Link for verification

### FINDING 2: [Next finding]
[Continue for all findings...]
```

**Step 2.2: Verify Evidence (Automated Quality Gate)**
For EVERY piece of data, automatically verify:
- Can attribute to specific source ✓
- Have sample size ✓
- Have URL for verification ✓
- Data correctly interpreted ✓

**If verification fails → Discard that finding and continue with verified data only**

---

### PHASE 3: AUTOMATED ARTICLE GENERATION

**Step 3.1: Generate Article Structure (Automated)**

```markdown
# [Title: Clear, benefit-focused, includes main keyword]

## Hook (2-3 paragraphs)
- Relatable parent scenario
- Acknowledge worry/question
- Preview research findings

## Evidence Section (3-4 paragraphs)
- Lead with numbers (prevalence, impact)
- Present key research findings
- Specific statistics with citations
- **BookBot mention 1**: Tie to research at BookBot/Flinders

## Translation Section (3-4 paragraphs)
- "What This Means" for parents
- Break down research implications
- Specific examples parents recognize
- **BookBot mention 2**: How BookBot addresses this

## Practical Strategies (5-7 bullet points)
- Evidence-based, actionable advice
- Start each with bold statement
- Explain WHY it works (cite research)
- Include realistic expectations
- **BookBot mention 3**: How BookBot implements this

## Encouragement/Conclusion (2-3 paragraphs)
- Normalize the struggle
- Reinforce that intervention helps
- Suggest next steps
- **BookBot mention 4**: CTA with link to https://www.bookbot.com
- End with engaging question for comments
```

**Step 3.2: Apply Voice Calibration (Automated)**

**Sanchari's Voice = Researcher-Experimenter**

✅ **Automatically include:**
- "I analyze data from thousands of kids..."
- "When I review the research..."
- "What I find most encouraging..."
- "Here's something that surprised me..." (NOT "diving into")
- Direct address: "Can your child hear that 'cat' and 'bat'..."
- Honest about limitations: "more isn't always better"
- Connect research to practical implications

❌ **Automatically avoid:**
- Generic blogger voice: "Research shows..." without specifics
- AI-sounding phrases: "Here's what surprised me when I started diving into..."
- Academic voice: "A landmark study by Colleagues et al."
- Overly maternal: "Your little one" or "sweet reader"

**Step 3.3: Enforce Citation Specificity (Automated)**

✅ **Every citation must have:**
- Author + year (e.g., "Wagner et al., 2020") OR
- Journal + year (e.g., "National Reading Panel, 2000")

❌ **Automatically reject:**
- "Multiple meta-analyses" → Must name specific study
- "Research shows" → Must specify which research
- "Studies indicate" → Must cite specific study
- "Meta-analyses, 2020-2022" → Must name actual meta-analysis

**If can't name specific study → Don't use that data**

**Step 3.4: Translate Jargon Immediately (Automated)**

**Rule:** Add "—meaning [plain language]" immediately after ANY technical term

Examples:
- "1.5 standard deviations below the mean—meaning they're reading significantly behind their peers"
- "effect sizes averaging 0.99—meaning children who received intervention made significantly more progress than those who didn't"
- "correlate at .603, which means about 36% of reading performance depends on..."

**Step 3.5: Integrate SEO Naturally (Automated)**

**If keywords provided:**
- Title: 1-2 primary keywords
- First paragraph: 1 primary keyword
- Subheadings: 2-3 keywords total
- Body: Distribute remaining keywords naturally
- **Never force keywords—if sounds unnatural, skip it**

**If no keywords provided:**
- Write naturally with topic-relevant terms
- Focus on clarity and readability over keyword density

**Step 3.6: Integrate BookBot Naturally (Automated)**

**Include 2-4 mentions:**

1. **After brain/neuroscience findings** → "This is exactly what we're working on at BookBot and Flinders University..."

2. **After timing/early intervention data** → "At BookBot, I analyze real reading data from thousands of children to identify early warning signs..."

3. **In strategies section** → "This is one reason we built BookBot—to make daily, intensive practice actually engaging..."

4. **In conclusion** → "That's what my research at BookBot and Flinders focuses on... [try BookBot](https://www.bookbot.com)"

**Voice for mentions:**
- Position as researcher explaining your work
- Not salesy—informational
- Connect to research just presented
- Show HOW BookBot implements the science

---

### PHASE 4: AUTOMATED QUALITY TESTING

**CRITICAL:** Run ALL tests below. If ANY test fails, execute self-correction loop.

---

#### TEST 1: Citation Audit

**Automated checks:**
- [ ] Every statistic has citation with author+year OR journal+year
- [ ] Zero vague citations ("research shows", "studies indicate", "multiple meta-analyses")
- [ ] References section matches all in-text citations
- [ ] All reference URLs are valid and accessible

**Status:** PASS / FAIL

**If FAIL:**
1. List specific violations (line numbers, problematic phrases)
2. Fix violations:
   - Add missing citations with specific author/year
   - Replace vague citations with specific sources
   - Add missing references
   - Fix broken URLs
3. Re-run Test 1
4. Repeat until PASS

---

#### TEST 2: Voice Authenticity Check

**Automated checks:**
- [ ] Contains "I analyze data..." or "When I review research..." markers (researcher voice present)
- [ ] Zero AI phrases detected: "diving into", "here's what surprised me when I started"
- [ ] Zero academic stuffiness: "A landmark study by Colleagues et al."
- [ ] Zero overly maternal language: "your little one", "sweet reader"
- [ ] Personal perspective present throughout (not just one paragraph)

**Status:** PASS / FAIL

**If FAIL:**
1. Identify problematic phrases (list them with line numbers)
2. Rewrite violations:
   - Replace AI phrases with natural researcher voice
   - Remove academic stuffiness, make conversational
   - Add personal perspective where missing
3. Re-run Test 2
4. Repeat until PASS

---

#### TEST 3: Jargon Translation Check

**Automated checks:**
- [ ] All technical terms followed by "—meaning [plain language]" translation
- [ ] No unexplained terms: "standard deviation", "effect size", "correlation", "percentile", etc.
- [ ] Reading level Grade 8-10 (use automated readability scorer)
- [ ] No dense academic paragraphs (paragraphs are 2-4 sentences)

**Status:** PASS / FAIL

**If FAIL:**
1. List untranslated jargon terms
2. Add "—meaning [plain language]" after each term
3. Simplify dense paragraphs
4. Re-run Test 3
5. Repeat until PASS

---

#### TEST 4: SEO Integration Check

**Automated checks:**
- [ ] Title under 70 characters
- [ ] Primary keyword in title (if keywords provided)
- [ ] Keywords integrated naturally (no keyword stuffing detected)
- [ ] No robotic phrasing: "If you're looking for X or Y or Z..."
- [ ] Excerpt/description compelling and under 150 characters
- [ ] Reading time calculated (word count / 180 = minutes)

**Status:** PASS / FAIL

**If FAIL:**
1. Identify forced/unnatural keyword placement
2. Rewrite sections for natural flow
3. Adjust title length if needed
4. Re-run Test 4
5. Repeat until PASS

---

#### TEST 5: BookBot Integration Check

**Automated checks:**
- [ ] 2-4 BookBot mentions present
- [ ] Mentions connected to research findings (not standalone marketing)
- [ ] Voice is informational (not salesy): "we're working on" not "you should buy"
- [ ] Maximum 1 direct CTA link (not pushy)
- [ ] Explains HOW BookBot implements the science discussed

**Status:** PASS / FAIL

**If FAIL:**
1. Identify problematic mentions (too salesy, disconnected from research, too many CTAs)
2. Revise mentions:
   - Tie to research findings
   - Remove sales language, make informational
   - Reduce CTAs to 1 maximum
3. Re-run Test 5
4. Repeat until PASS

---

#### TEST 6: Structural Completeness Check

**Automated checks:**
- [ ] Length: 900-1200 words
- [ ] All sections present: Hook, Evidence, Translation, Strategies, Conclusion
- [ ] 5-7 practical strategies provided
- [ ] References section complete with all sources cited in article
- [ ] YAML frontmatter complete: title, date, author, tags, excerpt, seo_keywords, reading_time

**Status:** PASS / FAIL

**If FAIL:**
1. Identify missing sections or incomplete elements
2. Add missing content
3. Re-run Test 6
4. Repeat until PASS

---

### SELF-CORRECTION LOOP LOGIC

```
Generate Article
↓
Run Test Suite (Tests 1-6)
↓
All Tests PASS? → YES → Proceed to Output
↓ NO
List Failed Tests
↓
For Each Failed Test:
  1. Identify specific violations
  2. Apply fixes
  3. Re-run that specific test
  4. If still FAIL → Repeat fix + re-test
  5. If PASS → Move to next failed test
↓
When All Tests PASS → Proceed to Output
```

**Maximum iterations:** 3 full cycles
- If tests still failing after 3 cycles → Output article with detailed error report for manual review

---

## OUTPUT GENERATION

Once ALL tests pass, automatically generate:

### OUTPUT 1: Final Article File

**Filename:** `[topic-slug]-article.md`

```markdown
---
title: "[Article Title]"
date: [YYYY-MM-DD]
author: Sanchari
tags: [tag1, tag2, tag3, tag4, tag5]
excerpt: "[150-character description]"
seo_keywords: [keyword1, keyword2, keyword3, ...]
reading_time: "[X] min read"
---

# [Article Title]

[Full article content]

---

## References

[All sources with full citations and links]
```

---

### OUTPUT 2: Process Documentation

**Filename:** `[topic-slug]-process-documentation.md`

```markdown
# Process Documentation: [Article Title]

**Date:** [YYYY-MM-DD]
**Workflow Version:** V4 (Automated)

---

## INPUT PROVIDED

**Topic:** [topic/URL provided]
**SEO Keywords:** [list or "None - natural optimization"]
**Target Outcome:** [educate/provide strategies/build trust/drive engagement]

---

## PHASE 1: RESEARCH GATHERED

### Sources Found (Auto)
1. [Source 1: Author, Year, Journal, URL]
   - Sample size: n=[?]
   - Key finding: [specific data]

2. [Source 2: Author, Year, Journal, URL]
   - Sample size: n=[?]
   - Key finding: [specific data]

[Continue for all sources...]

**Total sources:** [number]
**Sources rejected (quality filter):** [number and reasons]

---

## PHASE 2: EVIDENCE BANK BUILT

[Copy of complete evidence bank showing all findings with claims, sources, data, citations, URLs]

---

## PHASE 3: ARTICLE GENERATED

**Word count:** [number]
**Reading time:** [X] min
**SEO keywords used:** [number] / [total provided]
**BookBot mentions:** [number]

---

## PHASE 4: QUALITY TESTS EXECUTED

### Test 1: Citation Audit
**Status:** PASS / FAIL
**Iterations needed:** [number]
**Issues found:** [list if any]
**Fixes applied:** [list if any]

### Test 2: Voice Authenticity Check
**Status:** PASS / FAIL
**Iterations needed:** [number]
**Issues found:** [list if any]
**Fixes applied:** [list if any]

### Test 3: Jargon Translation Check
**Status:** PASS / FAIL
**Iterations needed:** [number]
**Issues found:** [list if any]
**Fixes applied:** [list if any]

### Test 4: SEO Integration Check
**Status:** PASS / FAIL
**Iterations needed:** [number]
**Issues found:** [list if any]
**Fixes applied:** [list if any]

### Test 5: BookBot Integration Check
**Status:** PASS / FAIL
**Iterations needed:** [number]
**Issues found:** [list if any]
**Fixes applied:** [list if any]

### Test 6: Structural Completeness Check
**Status:** PASS / FAIL
**Iterations needed:** [number]
**Issues found:** [list if any]
**Fixes applied:** [list if any]

---

## FINAL VALIDATION

**All Tests:** PASS ✅ / PARTIAL (see manual review needed) ⚠️

**Publication Ready:** YES / NEEDS MANUAL REVIEW

---

## METRICS SUMMARY

| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Word count | 900-1200 | [number] | ✅/❌ |
| Reading level | Grade 8-10 | [level] | ✅/❌ |
| Sources cited | 4-5+ | [number] | ✅/❌ |
| All stats cited | 100% | [%] | ✅/❌ |
| Specific citations | 100% | [%] | ✅/❌ |
| Jargon translated | 100% | [%] | ✅/❌ |
| Voice authenticity | Pass | [Pass/Fail] | ✅/❌ |
| SEO keywords | Natural | [Natural/Forced] | ✅/❌ |
| BookBot integration | 2-4 mentions | [number] | ✅/❌ |

---

## NEXT STEPS

- [ ] Human review of final article
- [ ] Publish to BookBot blog
- [ ] Track metrics (traffic, engagement, conversions)
- [ ] Use learnings for future articles

```

---

## SUCCESS CRITERIA

**Article is publication-ready when:**
- ✅ All 6 automated tests PASS
- ✅ Every statistic has verifiable source
- ✅ Reading level: Grade 8-10
- ✅ Length: 900-1200 words
- ✅ Voice: Sanchari (researcher-experimenter, warm but evidence-based)
- ✅ Natural SEO integration (no keyword stuffing)
- ✅ 2-4 BookBot mentions where they flow naturally
- ✅ All citations specific (no vague "research shows")
- ✅ All jargon translated immediately
- ✅ Personal perspective present throughout

---

## HOW TO USE THIS WORKFLOW

**For Adrian (or any content creator):**

1. **Copy this entire prompt** into Claude or any AI system

2. **Provide INPUT:**
   ```
   TOPIC: [your topic/URL here]
   SEO KEYWORDS: [optional - list keywords or skip]
   ```

3. **Run the workflow:** AI will automatically execute all 4 phases + testing

4. **Receive outputs:**
   - `[topic]-article.md` (publication-ready article)
   - `[topic]-process-documentation.md` (full process documentation)

5. **Review outputs:** Check that all tests passed, review article for final approval

6. **Publish:** If all tests passed and article looks good, publish to blog

---

## WORKFLOW VERSION HISTORY

**V1:** Manual workflow with fabricated citations ❌
**V2:** Real research, but generic voice and vague citations ⚠️
**V3:** Publication-ready with voice calibration, but manual testing ✅
**V4:** Fully automated with quality gates and self-correction ✅✅

**Current Version:** V4 (Automated)
**Date Created:** 2026-01-30
**Created By:** Sanchari 

---

**END OF AUTOMATED WORKFLOW V4**
