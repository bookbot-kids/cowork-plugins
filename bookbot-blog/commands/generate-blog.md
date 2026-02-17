# /generate-blog

Generate a publication-ready, research-backed blog post for the Bookbot website.

---

## Trigger

This command is the entry point for the Bookbot blog writing workflow. When invoked, follow the steps below in order.

---

## Pre-flight: Verify Configuration

Before starting, check that all required environment variables are set:

```bash
echo "GEMINI_API_KEY=${GEMINI_API_KEY:+set}" "GITHUB_TOKEN=${GITHUB_TOKEN:+set}" "BOOKBOT_AUTHOR=${BOOKBOT_AUTHOR:+set}"
```

If any variable shows as empty (not "set"), stop and tell the user:

> Some required configuration is missing. Please run `/setup` first to configure your API keys and author details.

Do NOT proceed with the workflow until all three variables are confirmed set.

---

## Step 1: Gather Input

Ask the employee:

> **What topic would you like to write about?**
>
> You can provide any of the following:
> - A general topic (e.g., "supporting struggling readers", "phonemic awareness")
> - An article URL you'd like to translate for a parent audience
> - A research paper URL or DOI
>
> Optionally, include any SEO keywords you'd like the article to target.

Wait for their response. Extract:
- **TOPIC**: The topic, URL, or DOI provided
- **SEO_KEYWORDS**: Any keywords mentioned (may be empty)

---

## Step 2: Execute the Blog Writing Workflow

Read and follow the `blog-writing-workflow.md` skill in full. Execute each phase in order:

1. **Phase 0** — Keyword optimization (if no keywords provided)
2. **Phase 1** — Research gathering
3. **Phase 2** — Evidence organization
4. **Phase 3** — Article generation (using `voice-guide.md` and `hugo-output-spec.md`)
5. **Phase 3.5** — Image generation (using `image-generation.md` and the Gemini API)
6. **Phase 4** — Quality testing with self-correction
7. **Phase 5** — Review, iteration, and publishing

---

## Step 3: Review Loop

After presenting the article and images:

- Wait for the reviewer's feedback
- If changes are requested, iterate on the article/images and re-run affected quality tests
- Continue until the reviewer gives explicit approval

---

## Step 4: Publish

Once approved:

1. Create a `blog/[slug]` branch in the `bookbot-www` Hugo website repository
2. Commit the article (`.md` file) to `content/updates/[category]/` and images to `assets/updates/`
3. Push the branch and open a pull request
4. Post a confirmation message with:
   - The PR link
   - The preview URL (branch deploy)
   - The production URL (once merged)

---

## Notes

- The full workflow typically produces: an article file, hero image, optional inline images, and process documentation
- Process documentation is for internal reference and is NOT committed to the Hugo repository
- If any phase encounters an unrecoverable error, present the partial output with a clear explanation of what went wrong and what needs manual attention
