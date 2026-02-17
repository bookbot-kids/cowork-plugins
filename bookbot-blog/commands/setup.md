# /setup

Check that all required tools, API keys, and configuration are in place, and guide the user through fixing anything that's missing.

---

## Required Configuration

| # | Check | Purpose |
|---|-------|---------|
| 1 | `GEMINI_API_KEY` env var | Image generation via Gemini API |
| 2 | GitHub auth (`gh` CLI or `GITHUB_TOKEN` env var) | Publishing articles to bookbot-www |
| 3 | `BOOKBOT_AUTHOR` env var | Author slug for article front matter |

---

## Step 1: Detect Operating System

Determine the user's OS (Windows, macOS, or Linux) to use the correct commands throughout setup.

---

## Step 2: Check Each Requirement

### 2a. GEMINI_API_KEY

Check if set:
- **Windows (PowerShell):** `echo $env:GEMINI_API_KEY`
- **macOS/Linux:** `echo $GEMINI_API_KEY`

If missing, ask the user for their Gemini API key and guide them to set it persistently (see "Setting Environment Variables" below).

If the user doesn't have a key, point them to: https://aistudio.google.com/apikey

### 2b. GitHub Authentication

The plugin publishes articles directly via the GitHub API — no local repository clone is needed. The user needs to authenticate with GitHub using ONE of these methods:

#### Option A: GitHub CLI (recommended)

Best for interactive use. One-time setup, token stored securely.

1. Check if installed:
   ```bash
   gh --version
   ```
   If not installed, direct the user to: https://cli.github.com/

2. Check if authenticated:
   ```bash
   gh auth status
   ```
   If not authenticated, guide them through:
   ```bash
   gh auth login
   ```

3. Verify repo access:
   ```bash
   gh repo view bookbot-kids/bookbot-www --json name
   ```

#### Option B: GitHub Personal Access Token

Best for users who don't want to install the `gh` CLI.

1. Go to: https://github.com/settings/tokens
2. Click **"Generate new token (classic)"**
3. Give it a name (e.g., "Bookbot Blog Plugin")
4. Select scope: **`repo`** (Full control of private repositories)
5. Click **Generate token** and copy it
6. Set as environment variable: `GITHUB_TOKEN` (see "Setting Environment Variables" below)
7. Verify access:
   ```bash
   # macOS/Linux
   curl -s -H "Authorization: Bearer $GITHUB_TOKEN" https://api.github.com/repos/bookbot-kids/bookbot-www --head | head -1

   # Windows (PowerShell)
   curl -s -H "Authorization: Bearer $env:GITHUB_TOKEN" https://api.github.com/repos/bookbot-kids/bookbot-www --head | Select-Object -First 1
   ```
   Should return `HTTP/2 200` if the token works and has repo access.

#### Detection Logic

During setup, check which auth method is available:
1. Try `gh auth status` — if it succeeds, use `gh api` for publishing
2. If `gh` is not available, check if `GITHUB_TOKEN` env var is set — if so, use `curl` with Bearer token
3. If neither is available, present both options and let the user choose

### 2c. BOOKBOT_AUTHOR

Check if set:
- **Windows (PowerShell):** `echo $env:BOOKBOT_AUTHOR`
- **macOS/Linux:** `echo $BOOKBOT_AUTHOR`

If missing, ask the user for their author slug. This will be used in the `author` field of every article's front matter.

Show existing authors for reference:
- `adrian-dewitts`
- `belinda-moore`
- `debs-moir`
- `jenni-mills`
- `shelly-dollosa`
- `sanchari-sengupta`

If the user is a new author, they can create a new slug (lowercase, hyphenated, e.g., `first-last`).

---

## Setting Environment Variables

For each missing variable, give the user the exact command to set it persistently:

### Windows (PowerShell)

```powershell
[System.Environment]::SetEnvironmentVariable("VARIABLE_NAME", "the-value", "User")
```

Tell the user they need to **restart their terminal** (or Claude Code) after running this for the change to take effect.

### macOS / Linux

```bash
echo 'export VARIABLE_NAME="the-value"' >> ~/.zshrc
source ~/.zshrc
```

---

## Step 3: Verify

After the user has set all variables and restarted their terminal, re-run all checks to confirm everything is working.

Once all checks pass, confirm:

> Setup complete. You're ready to use `/generate-blog`.

---

## Notes

- Never log or echo back the full API key value. When confirming a key is set, show only the first 5 characters followed by `...` (e.g., `AIza...`).
- For `GITHUB_TOKEN`, same rule — show only the first 5 characters followed by `...`.
- No local repository clone is needed — articles are published directly via the GitHub API.
- For `BOOKBOT_AUTHOR`, confirm the full value since it's not sensitive.
