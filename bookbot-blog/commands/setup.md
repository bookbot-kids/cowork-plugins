# /setup

Check that all required tools, API keys, and configuration are in place, and guide the user through fixing anything that's missing.

---

## Required Configuration

| # | Check | Purpose |
|---|-------|---------|
| 1 | `GEMINI_API_KEY` env var | Image generation via Gemini API |
| 2 | `gh` CLI installed and authenticated | Creating PRs on bookbot-www |
| 3 | `BOOKBOT_WWW_PATH` env var | Local path to the bookbot-www repository clone |
| 4 | `BOOKBOT_AUTHOR` env var | Author slug for article front matter |

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

### 2b. GitHub CLI (gh)

Check if installed:
```bash
gh --version
```

If not installed, direct the user to: https://cli.github.com/

Check if authenticated:
```bash
gh auth status
```

If not authenticated, guide them through:
```bash
gh auth login
```

Verify they have access to the bookbot-www repo:
```bash
gh repo view bookbot-kids/bookbot-www --json name
```

### 2c. BOOKBOT_WWW_PATH

Check if set:
- **Windows (PowerShell):** `echo $env:BOOKBOT_WWW_PATH`
- **macOS/Linux:** `echo $BOOKBOT_WWW_PATH`

If missing, ask the user for the local path to their `bookbot-www` repository clone. Verify the path is valid by checking that these directories exist:
- `[path]/content/updates/`
- `[path]/assets/updates/`

If the user hasn't cloned the repo yet, guide them:
```bash
gh repo clone bookbot-kids/bookbot-www
```

Then set `BOOKBOT_WWW_PATH` to the clone location.

### 2d. BOOKBOT_AUTHOR

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
- `sanchari`

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
- For `BOOKBOT_WWW_PATH`, show the full path since it's not sensitive.
- For `BOOKBOT_AUTHOR`, confirm the full value since it's not sensitive.
