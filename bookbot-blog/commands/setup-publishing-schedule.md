# /setup-publishing-schedule

Set up the **daily site rebuild** so future-dated articles publish automatically. Run this **once** (it's a one-time infrastructure step, separate from the per-article config in `/setup`).

---

## Why this is needed

Articles can be scheduled by giving them a future `date` in front matter. Hugo (built with no `--buildFuture` flag, timezone `Australia/Melbourne`) **hides** any future-dated article until its date passes — see `hugo-output-spec.md` → "Scheduling future posts".

But Cloudflare Pages only rebuilds the site when something triggers it (a git push, or a deploy hook). With no daily trigger, a scheduled article would sit invisible even after its date arrives. This command wires up a once-a-day rebuild so scheduled articles go live on time.

**Mechanism:** a **Cloudflare Pages Deploy Hook** (a URL that triggers a production build) + a **daily Claude Cowork routine** that POSTs to it. No empty commits, no git-history noise.

---

## Step 1: Create the Cloudflare Deploy Hook (one time, in the dashboard)

The owner does this in the Cloudflare dashboard:

1. Go to **Cloudflare dashboard → Workers & Pages → `bookbot-www` → Settings → Builds**.
2. Under **Deploy hooks**, click **Add deploy hook**.
3. Name it `Daily scheduled rebuild`, set the branch to **`main`**, and save.
4. Copy the generated URL. It looks like:
   `https://api.cloudflare.com/client/v4/pages/webhooks/deploy_hooks/<token>`

Treat this URL as a **secret** — anyone with it can trigger a build.

## Step 2: Test the hook

Confirm a POST triggers a build:

```bash
curl -fsS -X POST "<DEPLOY_HOOK_URL>"
```

A successful response returns a JSON body with an `id`. Check **Workers & Pages → `bookbot-www` → Deployments** — a new deployment should appear. (Optionally save the URL as `CLOUDFLARE_DEPLOY_HOOK` in `.claude/settings.local.json` for convenient manual testing later.)

## Step 3: Create the daily Cowork routine

Use the `/schedule` skill to create a recurring cloud agent (routine):

- **Cadence:** daily at **00:15 Australia/Melbourne** (just after midnight, so an article whose date "becomes today" publishes promptly).
- **Action:** POST to the deploy hook and report the result. The routine prompt should be self-contained, e.g.:

  > Trigger the daily Bookbot site rebuild so any newly-due scheduled articles go live. Run:
  > `curl -fsS -X POST "<DEPLOY_HOOK_URL>"`
  > Report whether it succeeded (HTTP 200 and a deployment id) or failed.

The deploy hook URL lives inside the routine definition (it's a capability token — keep the routine private).

## Step 4: Verify

- Run `/schedule` and confirm the routine is listed with the daily Melbourne cadence.
- Trigger the routine once manually and confirm it returns success and a new deployment appears in Cloudflare.
- End-to-end: publish a test article dated **tomorrow**, confirm it is NOT live today, and confirm it appears after the routine fires past its date.

---

## Notes

- This only needs to run **once**. After setup, scheduling an article is just a matter of setting a future `date` (handled by `/generate-blog`).
- If you'd rather keep the trigger entirely inside Cloudflare (no dependency on Cowork), an alternative is a **Cloudflare Worker Cron Trigger** that POSTs to the same deploy hook. The routine approach is the default because it needs no extra Cloudflare Worker.
- Do not use an empty daily git commit for this — it pollutes the repo history. The deploy hook achieves the same rebuild cleanly.
