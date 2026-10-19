# Live Instagram stats — simple setup

Your page shows numbers from `stats.json`. A GitHub job refreshes that file every
6 hours using your Instagram token. The token lives in a GitHub **secret**, so it
is never in your page, your code, or your repo.

## Files

| File | What it does |
|------|--------------|
| `index.html` | Your page. Reads `stats.json` and fills in the numbers. |
| `stats.json` | The latest stats (public, numbers only). |
| `scripts/update-stats.mjs` | Fetches stats from Instagram. |
| `.github/workflows/update-stats.yml` | Runs the script every 6 hours. |

Upload all of them to your GitHub repo (keep the same folder layout).

## Step 1: Get an Instagram token (one time)

You need an Instagram **Business or Creator** account.

1. Go to https://developers.facebook.com and create an app (type: Business).
2. Add the **Instagram** product, then choose **API setup with Instagram login**.
3. Add your Instagram account and generate a token.
   Permissions needed: `instagram_business_basic` and `instagram_business_manage_insights`.
4. Copy the **long-lived** token (lasts 60 days; the job refreshes it automatically).

## Step 2: Save the token as a secret

1. In your GitHub repo: **Settings → Secrets and variables → Actions**.
2. Click **New repository secret**.
3. Name: `IG_ACCESS_TOKEN`
4. Value: paste your token. Save.

Never paste the token into `index.html`, chat, or any file.

## Step 3: Allow the job to save stats

**Settings → Actions → General → Workflow permissions →** choose
**Read and write permissions** → Save.

## Step 4: Run it once

**Actions tab → Update Instagram stats → Run workflow.**
After a minute, `stats.json` updates and your page shows the new numbers.
From then on it runs by itself every 6 hours.

## Good to know

- **Engagement rate** is calculated as interactions ÷ reach, so it may differ a
  little from the number in Instagram's own dashboard.
- If Instagram or the token fails, the job keeps the last good numbers; your page
  never goes blank.
- **Token expiry:** if the job starts failing after ~60 days, generate a new token
  and update the secret.
- If the token is ever exposed, revoke it in the Meta developer console and
  replace the secret.
