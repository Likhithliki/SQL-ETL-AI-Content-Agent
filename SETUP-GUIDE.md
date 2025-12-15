# Setup Guide: SQL-ETL-AI-Content-Agent

## Complete Step-by-Step Setup for YouTube Shorts + LinkedIn Auto-Posting

---

## Phase 1: Create Accounts (15 minutes)

### 1.1 n8n Cloud Account

1. Go to **https://app.n8n.cloud**
2. Click **"Start free trial"**
3. Sign up with email or Google
4. Verify your email
5. Accept terms and create account

✅ You now have n8n Cloud account

---

### 1.2 OpenRouter Account (for LLM)

1. Go to **https://openrouter.ai**
2. Click **"Sign Up"**
3. Use email or GitHub to register
4. Verify email
5. Go to **https://openrouter.ai/keys**
6. Copy your API key (save in a text editor)

✅ You have OpenRouter API Key

---

### 1.3 Blotato Account (for Video Generation)

1. Go to **https://www.blotato.ai**
2. Click **"Sign Up"** or **"Try For Free"**
3. Create account with email
4. Go to **Dashboard** → **API Keys**
5. Copy your API key (save in text editor)
6. Note your free credits (usually 5-10)

✅ You have Blotato API Key

---

### 1.4 Google Cloud Console (for YouTube)

1. Go to **https://console.cloud.google.com**
2. Create a new project
   - Click **Select Project** → **New Project**
   - Name it: `SQL-ETL-Content-Agent`
   - Click **Create**
3. Enable YouTube API
   - Left sidebar: **APIs & Services** → **Library**
   - Search **"YouTube Data API v3"**
   - Click it → **Enable**
4. Create OAuth credentials
   - **APIs & Services** → **Credentials**
   - Click **Create Credentials** → **OAuth 2.0 Client ID**
   - Choose **Desktop Application**
   - Click **Create**
   - Download as JSON (you'll use this in n8n)

✅ YouTube OAuth configured

---

### 1.5 LinkedIn Developer App

1. Go to **https://www.linkedin.com/developers/apps**
2. Click **Create app**
3. Fill in:
   - **App name**: SQL-ETL-Content-Agent
   - **LinkedIn Page**: (choose your page or create one)
   - **App logo**: (upload any logo)
   - Accept terms
4. In **Settings** tab:
   - Authorized redirect URLs: `https://app.n8n.cloud/`
   - Save
5. Go to **Auth** tab
   - Copy **Client ID** and **Client Secret** (save these)

✅ LinkedIn app configured

---

## Phase 2: Import Workflow into n8n (5 minutes)

### 2.1 Access n8n

1. Log into **https://app.n8n.cloud**
2. Go to **Workflows**
3. Click **Import**

### 2.2 Import JSON

1. Open file **`n8n-workflow.json`** from this repo
2. Copy all content
3. In n8n import dialog, paste the JSON
4. Click **Import**

✅ Workflow imported into n8n

---

## Phase 3: Add Credentials (10 minutes)

### 3.1 OpenRouter API Key

1. In n8n, open the imported workflow
2. Find node **"AI Script Generator"**
3. Edit the HTTP request
4. In **Headers** section, find:
   ```
   Authorization: Bearer YOUR_OPENROUTER_API_KEY
   ```
5. Replace `YOUR_OPENROUTER_API_KEY` with your actual key from OpenRouter
6. Click **Save**

### 3.2 Blotato API Key

1. Find node **"Create Video via Blotato"** (if present in your workflow)
2. Edit the HTTP request
3. In **Headers**, find:
   ```
   Authorization: Bearer YOUR_BLOTATO_API_KEY
   ```
4. Replace with your Blotato key
5. Click **Save**

### 3.3 YouTube OAuth

1. Find node **"YouTube Shorts Upload"**
2. Click the **Credential** field
3. Click **Create Credential**
4. Select **Google OAuth 2.0**
5. Click **Authenticate**
6. Sign in with your Google account that owns YouTube channel
7. Grant YouTube upload permissions
8. OAuth token is automatically saved

### 3.4 LinkedIn OAuth

1. Find node **"LinkedIn Video Post"**
2. Click the **Credential** field
3. Click **Create Credential**
4. Paste your LinkedIn **Client ID** and **Client Secret**
5. Click **Authenticate**
6. Sign in with your LinkedIn account
7. Grant post-sharing permissions
8. OAuth token is automatically saved

✅ All credentials added

---

## Phase 4: Test Your Workflow (5 minutes)

### 4.1 Prepare Test

1. Open your workflow in n8n
2. Find **"Set Topic"** node
3. Change the topic value to: `Window Functions in SQL`
4. Click **Save**

### 4.2 Execute

1. Click **Execute Workflow** (Play button, top right)
2. Watch the execution log:
   - ✅ Script generated (check console output)
   - ✅ Video job created
   - ✅ Video rendering (may take 1-3 minutes)
   - ✅ Video downloaded
   - ✅ Posted to YouTube
   - ✅ Posted to LinkedIn

### 4.3 Verify

1. Go to **https://youtube.com/me/shorts** → Check for your new Short
2. Go to **https://linkedin.com/feed** → Check for your new post
3. Both should be visible within 5 minutes

✅ Workflow is live!

---

## Phase 5: Automate with Cron (Optional, 5 minutes)

Instead of manual triggers, run daily at 7 PM IST:

### 5.1 Replace Manual Trigger with Cron

1. Delete **"Manual Trigger"** node
2. Add new node: **Cron**
3. Set:
   - Trigger mode: **Every day**
   - Time: **19:00** (7 PM)
   - Timezone: **Asia/Kolkata** (IST)
4. Click **Save**

### 5.2 Add Topic List

1. Add new node: **Google Sheets** (optional)
2. Point to a spreadsheet with your daily topics
3. Set "Set Topic" node to read from the sheet
4. Deploy workflow

Now your agent will generate and post daily!

---

## Troubleshooting

### Problem: OpenRouter returns 401 error

**Solution:**
- Double-check your API key is copied correctly
- Ensure no extra spaces or line breaks in the key
- Go to https://openrouter.ai/keys and regenerate if needed

---

### Problem: Blotato video API returns error

**Solution:**
- Check https://www.blotato.ai/dashboard for remaining credits
- Free tier has limited renders; upgrade if needed
- Verify API endpoint is correct in workflow

---

### Problem: YouTube upload fails with 403 error

**Solution:**
- Verify Google account is a YouTube channel owner
- Go to https://myaccount.google.com/permissions → YouTube → verify access
- Re-authenticate: remove credential and click "Create Credential" again

---

### Problem: LinkedIn post doesn't appear

**Solution:**
- Check LinkedIn app **Settings** → Authorized Redirect URLs includes `https://app.n8n.cloud/`
- Verify your LinkedIn account has posting permissions
- Check if text + hashtags exceed LinkedIn's character limit (2,000 chars)
- Re-authenticate LinkedIn credential

---

### Problem: Script generation is very slow

**Solution:**
- OpenRouter may have high load; wait 1-2 minutes and retry
- Try switching LLM model in n8n from `gpt-4o-mini` to `gpt-3.5-turbo` (faster, cheaper)
- Check OpenRouter status at https://openrouter.ai/status

---

## Cost Optimization

- **OpenRouter**: Use `gpt-3.5-turbo` instead of `gpt-4o-mini` to cut costs by 90%
- **Blotato**: 5-10 free credits per month; $0.50-1 per video after
- **n8n Cloud**: Free tier supports up to 5,000 monthly executions
- **YouTube + LinkedIn**: Always free for posting

**Monthly cost for 20 videos/month:**
- LLM: $2-5
- Video generation: $10-20 (if using paid Blotato)
- n8n: Free (under 5,000 executions)
- **Total: $12-25/month**

---

## Next Steps

1. ✅ Complete all setup phases above
2. ✅ Run first test workflow
3. ✅ Schedule daily runs if desired
4. ✅ Update topic list every week
5. ✅ Monitor videos for engagement
6. ✅ Refine AI prompts based on performance

---

## Support

For issues not covered here:

1. Check n8n logs: Workflow → Executions → click failed run → see error
2. Review API documentation:
   - OpenRouter: https://openrouter.ai/docs
   - Blotato: https://www.blotato.ai/docs
   - YouTube: https://developers.google.com/youtube/v3
   - LinkedIn: https://docs.microsoft.com/linkedin/shared/api-reference
3. Test API keys individually in Postman or cURL
4. Ask in n8n Community: https://community.n8n.io

---

**You're all set! Your SQL/ETL content agent is ready to run!** 🚀
