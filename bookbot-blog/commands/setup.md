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
| 5 | `FACEBOOK_PAGE_TOKEN` | Long-lived Page Access Token for Facebook and Instagram posting |
| 6 | `FACEBOOK_PAGE_ID` | Facebook Page ID for the Bookbot page |
| 7 | `INSTAGRAM_ACCOUNT_ID` | Instagram Business Account ID (linked to the Facebook Page) |
| 8 | `PINTEREST_TOKEN` | OAuth 2.0 Bearer token for Pinterest API |
| 9 | `PINTEREST_BOARD_ID` | Pinterest board ID to pin articles to |
| 10 | `LINKEDIN_TOKEN` | OAuth 2.0 Bearer token for LinkedIn API |
| 11 | `LINKEDIN_ORG_ID` | LinkedIn Organisation (Company Page) ID |

All 11 variables are required.

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

### 1e. FACEBOOK_PAGE_TOKEN, FACEBOOK_PAGE_ID, and INSTAGRAM_ACCOUNT_ID

These three variables use the same Meta developer app. Facebook and Instagram share the same token.

Check:
```bash
echo "${FACEBOOK_PAGE_TOKEN:+set}" "${FACEBOOK_PAGE_ID:+set}" "${INSTAGRAM_ACCOUNT_ID:+set}"
```

If missing, guide the user through setup:

1. Go to https://developers.facebook.com/ and create an app (type: **"Business"**)
2. Add the **Facebook Login** and **Instagram Graph API** products to the app
3. Under **App Review > Permissions and Features**, request these permissions:
   - `pages_manage_posts` — post to your Facebook Page
   - `pages_read_engagement` — read Page engagement data
   - `pages_show_list` — list pages you manage
   - `instagram_basic` — access Instagram account info
   - `instagram_content_publish` — post to Instagram
4. Go to **Tools > Graph API Explorer**:
   - Select your app from the dropdown
   - Select your Page
   - Generate a **User Access Token** with the permissions above
5. **Convert to a long-lived token** (the user token from step 4 expires in 1 hour):
   ```bash
   curl -s "https://graph.facebook.com/v22.0/oauth/access_token?grant_type=fb_exchange_token&client_id=YOUR_APP_ID&client_secret=YOUR_APP_SECRET&fb_exchange_token=SHORT_LIVED_USER_TOKEN" | jq -r '.access_token'
   ```
6. **Get the Page Access Token** from the long-lived user token:
   ```bash
   curl -s "https://graph.facebook.com/v22.0/me/accounts?access_token=LONG_LIVED_USER_TOKEN" | jq '.data[] | {name, id, access_token}'
   ```
   This returns a Page Access Token that does not expire (unless permissions are revoked). The `id` field is the **Page ID**.
7. Save `FACEBOOK_PAGE_TOKEN` (the Page Access Token from step 6) and `FACEBOOK_PAGE_ID` (the `id` from step 6).
8. **Get Instagram Account ID:**
   ```bash
   curl -s "https://graph.facebook.com/v22.0/$FACEBOOK_PAGE_ID?fields=instagram_business_account&access_token=$FACEBOOK_PAGE_TOKEN" | jq -r '.instagram_business_account.id'
   ```
   This requires the Instagram Business or Creator account to be linked to the Facebook Page. Save the result as `INSTAGRAM_ACCOUNT_ID`.

Verify Facebook access:
```bash
curl -s "https://graph.facebook.com/v22.0/$FACEBOOK_PAGE_ID?fields=name&access_token=$FACEBOOK_PAGE_TOKEN" | jq '.name'
```
Should return the Page name.

Verify Instagram access:
```bash
curl -s "https://graph.facebook.com/v22.0/$INSTAGRAM_ACCOUNT_ID?fields=username&access_token=$FACEBOOK_PAGE_TOKEN" | jq '.username'
```
Should return the Instagram username.

### 1f. PINTEREST_TOKEN and PINTEREST_BOARD_ID

Check:
```bash
echo "${PINTEREST_TOKEN:+set}" "${PINTEREST_BOARD_ID:+set}"
```

If missing, guide the user through setup:

1. Go to https://developers.pinterest.com/ and create an app
2. Under **App settings**, note the App ID and App Secret
3. Generate a token via the **Apps > Generate Token** feature in the developer dashboard, or via the OAuth 2.0 flow
   - Required scopes: `pins:write`, `boards:read`
4. **Get the Board ID** — list your boards:
   ```bash
   curl -s "https://api.pinterest.com/v5/boards" \
     -H "Authorization: Bearer $PINTEREST_TOKEN" | jq '.items[] | {id, name}'
   ```
   Pick the board ID for the board you want to pin articles to.

Save `PINTEREST_TOKEN` and `PINTEREST_BOARD_ID`.

Verify access:
```bash
curl -s "https://api.pinterest.com/v5/user_account" \
  -H "Authorization: Bearer $PINTEREST_TOKEN" | jq '.username'
```
Should return your Pinterest username.

**Note:** Pinterest tokens are valid for 30 days. Use the refresh token flow to renew before expiry.

### 1g. LINKEDIN_TOKEN and LINKEDIN_ORG_ID

Check:
```bash
echo "${LINKEDIN_TOKEN:+set}" "${LINKEDIN_ORG_ID:+set}"
```

If missing, guide the user through setup:

1. Go to https://www.linkedin.com/developers/ and create an app
2. Under **Products**, request access to the **Community Management API**
3. Under **Auth**, note the Client ID and Client Secret
4. Generate a token via the OAuth 2.0 3-legged flow:
   - Required scope: `w_organization_social`
   - Authorisation URL: `https://www.linkedin.com/oauth/v2/authorization`
   - Token URL: `https://www.linkedin.com/oauth/v2/accessToken`
5. **Get the Organisation ID:**
   ```bash
   curl -s "https://api.linkedin.com/rest/organizationAcls?q=roleAssignee" \
     -H "Authorization: Bearer $LINKEDIN_TOKEN" \
     -H "LinkedIn-Version: 202501" \
     -H "X-Restli-Protocol-Version: 2.0.0" | jq '.elements[].organization'
   ```
   The organisation URN contains the ID (e.g., `urn:li:organization:12345678` — the ID is `12345678`).

Save `LINKEDIN_TOKEN` and `LINKEDIN_ORG_ID`.

Verify access:
```bash
curl -s "https://api.linkedin.com/rest/organizations/$LINKEDIN_ORG_ID" \
  -H "Authorization: Bearer $LINKEDIN_TOKEN" \
  -H "LinkedIn-Version: 202501" \
  -H "X-Restli-Protocol-Version: 2.0.0" | jq '.localizedName'
```
Should return the organisation name.

**Note:** LinkedIn tokens expire after 60 days. The user will need to refresh periodically via the OAuth flow.

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
    "DATAFORSEO_AUTH": "base64-encoded-login:password",
    "FACEBOOK_PAGE_TOKEN": "the-token",
    "FACEBOOK_PAGE_ID": "the-page-id",
    "INSTAGRAM_ACCOUNT_ID": "the-ig-id",
    "PINTEREST_TOKEN": "the-token",
    "PINTEREST_BOARD_ID": "the-board-id",
    "LINKEDIN_TOKEN": "the-token",
    "LINKEDIN_ORG_ID": "the-org-id"
  }
}
```

Variables saved here are available as standard environment variables (`$GEMINI_API_KEY`, etc.) in all subsequent commands — no restart required.

---

## Step 2: Verify

After saving all variables, re-check each one to confirm it's set:

```bash
# Core (blog generation & publishing)
echo "GEMINI_API_KEY=${GEMINI_API_KEY:+set}"
echo "GITHUB_TOKEN=${GITHUB_TOKEN:+set}"
echo "BOOKBOT_AUTHOR=$BOOKBOT_AUTHOR"
echo "DATAFORSEO_AUTH=${DATAFORSEO_AUTH:+set}"

# Social media
echo "FACEBOOK_PAGE_TOKEN=${FACEBOOK_PAGE_TOKEN:+set}"
echo "FACEBOOK_PAGE_ID=${FACEBOOK_PAGE_ID:+set}"
echo "INSTAGRAM_ACCOUNT_ID=${INSTAGRAM_ACCOUNT_ID:+set}"
echo "PINTEREST_TOKEN=${PINTEREST_TOKEN:+set}"
echo "PINTEREST_BOARD_ID=${PINTEREST_BOARD_ID:+set}"
echo "LINKEDIN_TOKEN=${LINKEDIN_TOKEN:+set}"
echo "LINKEDIN_ORG_ID=${LINKEDIN_ORG_ID:+set}"
```

Once all 11 checks pass, confirm:

> Setup complete. You're ready to use `/generate-blog`.

---

## Notes

- Never log or echo back the full API key or token value. When confirming a key is set, show only the first 5 characters followed by `...` (e.g., `AIza...`).
- Same rule for all tokens: `GITHUB_TOKEN`, `DATAFORSEO_AUTH`, `FACEBOOK_PAGE_TOKEN`, `PINTEREST_TOKEN`, `LINKEDIN_TOKEN` — first 5 characters + `...`.
- For IDs (`FACEBOOK_PAGE_ID`, `INSTAGRAM_ACCOUNT_ID`, `PINTEREST_BOARD_ID`, `LINKEDIN_ORG_ID`), confirm the full value since they are not sensitive.
- For `BOOKBOT_AUTHOR`, confirm the full value since it's not sensitive.
- No local repository clone is needed — articles are published directly via the GitHub API.
- Do NOT write variables to `~/.zshrc` or shell profiles — use `.claude/settings.local.json` only.
- LinkedIn tokens expire after 60 days and Pinterest tokens after 30 days. If social media posting fails with auth errors, re-run `/setup` to refresh the expired token.
