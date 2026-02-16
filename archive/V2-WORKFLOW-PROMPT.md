# VERSION 2 WORKFLOW PROMPT
## Evidence-Based Research Approach (Without Voice Refinement)

**Purpose:** Generate blog post with REAL research citations (no fabrication), but voice calibration applied AFTER writing, not during.

**What this version does well:** Prevents fabricated citations
**What this version lacks:** Voice calibration, citation specificity, jargon translation, BookBot integration

---

## YOUR TASK

You are writing a blog post for BookBot's parent/teacher audience. Follow this process:

### INPUTS NEEDED:
1. **TOPIC**: [What parent question are you answering?]
2. **SEO KEYWORDS**: [8-12 keywords]
3. **TARGET**: [Educate? Provide strategies? Build trust?]

---

## PHASE 1: RESEARCH GATHERING (MANDATORY)

### Step 1: Conduct Web Searches

Search for:
- `[topic] research 2024 2025 peer-reviewed`
- `[topic] meta-analysis`
- `[topic] intervention studies`

**Prioritize:**
- PMC (PubMed Central) articles
- Frontiers journals
- National Reading Panel reports
- .edu domains

**Minimum: 4-5 credible sources**

### Step 2: Fetch and Read Papers

Use WebFetch to access full papers and extract:
- Title, authors, year, journal
- Sample size (n=?)
- Specific findings (percentages, effect sizes)
- Key quotes

### Step 3: Quality Check

❌ **NEVER use if:**
- Can't access full paper
- Sample too small (n<50)
- Not peer-reviewed

✅ **Prefer:**
- Large samples (n>100)
- Published within 10 years
- Meta-analyses

---

## PHASE 2: EVIDENCE BANK (MANDATORY)

Build evidence bank BEFORE writing:

```markdown
## EVIDENCE BANK

### FINDING 1: [Description]
- **CLAIM**: What you'll say in article
- **SOURCE**: Author, Year, Journal
- **DATA**: Exact statistic + sample size
- **CITATION**: How to cite in-text
- **URL**: Link for verification

### FINDING 2: [Next finding]
[Repeat...]
```

**Verification checklist:**
- [ ] Can attribute to specific source
- [ ] Have sample size
- [ ] Have URL for verification
- [ ] Data correctly interpreted

**If cannot check all boxes → DO NOT use that finding**

---

## PHASE 3: ARTICLE GENERATION

### Structure:

```markdown
# Title: Clear, includes main keyword

## Hook (2-3 paragraphs)
- Relatable parent scenario
- Acknowledge worry
- Preview research findings

## Evidence Section (3-4 paragraphs)
- Lead with numbers
- Present key research
- Use statistics with citations

## Translation Section (3-4 paragraphs)
- "What This Means" for parents
- Break down implications
- Specific examples

## Practical Strategies (5-7 bullets)
- Evidence-based advice
- Explain WHY it works
- Cite research

## Conclusion (2-3 paragraphs)
- Normalize the struggle
- Reinforce intervention helps
- Suggest next steps
- Engaging question for comments
```

### Citation Rules:
- Every statistic must have citation
- Format: (Author et al., Year)
- References section at end

### SEO Integration:
- 8-12 keywords total
- Place in title, first paragraph, subheadings, body
- Natural placement (don't force)

---

## PHASE 4: VERIFICATION

### Citation Audit:
- [ ] Every statistic has citation
- [ ] Every citation has author/year
- [ ] References section matches in-text citations
- [ ] All links work

### Readability Check:
- [ ] Grade 8-10 reading level
- [ ] Paragraphs 2-4 sentences
- [ ] Subheadings every 3-4 paragraphs

### SEO Check:
- [ ] Title under 70 characters
- [ ] Primary keyword in title
- [ ] 8-12 keywords used naturally
- [ ] Reading time calculated

---

## OUTPUT FORMAT

```markdown
---
title: "[Article Title]"
date: [YYYY-MM-DD]
author: Sanchari
tags: [tag1, tag2, tag3, tag4, tag5]
excerpt: "[150-character description]"
seo_keywords: [keyword1, keyword2, ...]
reading_time: "[X] min read"
---

# [Article Title]

[Full article content]

---

## References

[All sources with links]
```

---

## KNOWN LIMITATIONS OF VERSION 2

This workflow will produce articles with:
- ✅ Real, verifiable citations
- ❌ Generic blogger voice (not Sanchari's voice)
- ❌ Vague citations allowed ("multiple meta-analyses")
- ❌ Technical jargon not translated
- ❌ No BookBot integration
- ❌ AI-sounding phrases present

**These issues require voice calibration AFTER writing (Version 3 fixes this by calibrating DURING generation)**

---

## SUCCESS CRITERIA FOR VERSION 2

- [ ] 900-1200 words
- [ ] 4-5 real sources cited
- [ ] All statistics have citations
- [ ] References section complete
- [ ] No fabricated data
- [ ] Grade 8-10 reading level

**Expected result:** Real research, but needs voice and specificity refinement
