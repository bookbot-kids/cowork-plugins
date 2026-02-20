# /setup

Check that all required configuration is in place, and guide the user through fixing anything that's missing.

---

## Required Configuration

| # | Variable | Purpose |
|---|----------|---------|
| 1 | `GEMINI_API_KEY` | Image generation via Gemini API |
| 2 | `GITHUB_TOKEN` | Publishing articles to bookbot-www via GitHub API |
| 3 | `BOOKBOT_AUTHOR` | Author slug for article front matter |
| 4 | `DATAFORSEO_AUTH` | DataForSEO API credentials (Base64 encoded `login:password`) for keyword and FAQ research |

All 4 variables are required.

All variables are stored in `.claude/settings.local.json` (gitignored, per-machine). They persist across sessions, sandbox restarts, and plugin updates automatically.

---

## Step 1: Check Each Requirement

For each variable, check if it's already set by running `echo $VARIABLE_NAME`. If it returns a value, it's configured. If empty, follow the setup steps below and then save it (see "Saving Variables" at the end).

### 1a. GEMINI_API_KEY

Check: `echo $GEMINI_API_KEY`

If missing, ask the user for their Gemini API key.

If the user doesn't have a key, point them to: https://aistudio.google.com/apikey

### 1b. GITHUB_TOKEN

Check: `echo $GITHUB_TOKEN`

The plugin publishes articles directly via the GitHub API using `curl` — no local repository clone or CLI tools are needed. The user needs a GitHub Personal Access Token.

If missing, guide the user through creating one:

1. Go to: https://github.com/settings/tokens
2. Click **"Generate new token (classic)"**
3. Give it a name (e.g., "Bookbot Blog Plugin")
4. Select scopes: **`repo`** (Full control of private repositories) and **`read:org`** (Read org membership — required to access the bookbot-kids organization)
5. Click **Generate token** and copy it

After the user provides the token, save it (see "Saving Variables" below), then verify access:
```bash
curl -s -H "Authorization: token $GITHUB_TOKEN" https://api.github.com/repos/bookbot-kids/bookbot-www --head | head -1
```
Should return `HTTP/2 200` if the token works and has repo access.

### 1c. BOOKBOT_AUTHOR

Check: `echo $BOOKBOT_AUTHOR`

If missing, ask the user for their author slug. This will be used in the `author` field of every article's front matter.

Show existing authors for reference:
- `adrian-dewitts`
- `belinda-moore`
- `debs-moir`
- `jenni-mills`
- `shelly-dollosa`
- `sanchari-sengupta`

If the user is a new author, they can create a new slug (lowercase, hyphenated, e.g., `first-last`).

### 1d. DATAFORSEO_AUTH

Check: `echo "${DATAFORSEO_AUTH:+set}"`

If missing, ask the user for their DataForSEO Base64 auth string. This is the Base64 encoding of `login:password` and can be found or generated from their DataForSEO account at https://app.dataforseo.com/api-access.

If they only have their login and password, generate the token for them:
```bash
echo -n "their-login@email.com:their-password" | base64
```

Save the resulting string as `DATAFORSEO_AUTH`.

---

## Saving Variables

For each missing variable, save it to `.claude/settings.local.json` in the project root. This file is gitignored and persists across sessions, sandbox restarts, and plugin updates.

**How to save:** Read `.claude/settings.local.json`, then use the Edit tool to add or update the `env` block with the new variable. If the `env` block doesn't exist yet, create it.

Target format:
```json
{
  "permissions": { ... },
  "env": {
    "GEMINI_API_KEY": "the-key",
    "GITHUB_TOKEN": "the-token",
    "BOOKBOT_AUTHOR": "the-slug",
    "DATAFORSEO_AUTH": "base64-encoded-login:password"
  }
}
```

Variables saved here are available as standard environment variables (`$GEMINI_API_KEY`, etc.) in all subsequent commands — no restart required.

---

## Step 2: Verify

After saving all variables, re-check each one to confirm it's set:

```bash
echo $GEMINI_API_KEY
echo $GITHUB_TOKEN
echo $BOOKBOT_AUTHOR
echo "${DATAFORSEO_AUTH:+set}"
```

Once all checks pass, confirm:

> Setup complete. You're ready to use `/generate-blog`.

---

## Notes

- Never log or echo back the full API key value. When confirming a key is set, show only the first 5 characters followed by `...` (e.g., `AIza...`).
- For `GITHUB_TOKEN` and `DATAFORSEO_AUTH`, same rule — show only the first 5 characters followed by `...`.
- For `BOOKBOT_AUTHOR`, confirm the full value since it's not sensitive.
- No local repository clone is needed — articles are published directly via the GitHub API.
- Do NOT write variables to `~/.zshrc` or shell profiles — use `.claude/settings.local.json` only.
