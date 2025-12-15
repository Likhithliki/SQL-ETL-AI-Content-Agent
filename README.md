# SQL-ETL-AI-Content-Agent

🤖 **Automate your SQL/ETL content creation** - Generate 60-second educational videos and auto-post to YouTube Shorts + LinkedIn

## 🎯 What It Does

1. **Input**: You send a topic (e.g., "INNER vs LEFT JOIN in SQL")
2. **AI Script Generation**: Creates a 60-second beginner-friendly script with code snippets
3. **Video Creation**: Generates a vertical 9:16 video with voiceover and animations
4. **Auto-Post**: Posts simultaneously to YouTube Shorts + LinkedIn with hashtags

**Time to full automation**: ~5 minutes per topic

---

## 🛠️ Tech Stack

| Component | Service | Purpose |
|-----------|---------|----------|
| **Workflow Automation** | n8n Cloud | Orchestrate the entire pipeline |
| **LLM** | OpenRouter / OpenAI | Generate scripts & captions |
| **Video Generation** | Blotato | Create 60s vertical videos |
| **YouTube** | YouTube API | Post Shorts |
| **LinkedIn** | LinkedIn Graph API | Post videos |
| **Storage** | GitHub | Configuration & credentials |

---

## 📋 Setup Instructions

### Step 1: Create Free Accounts

```bash
# Required (all free tier available)
1. n8n Cloud - https://app.n8n.cloud
2. OpenRouter - https://openrouter.ai
3. Blotato - https://www.blotato.ai
4. Google Cloud Console - https://console.cloud.google.com (for YouTube OAuth)
5. LinkedIn Developer App - https://www.linkedin.com/developers/apps
```

### Step 2: Get API Keys

**OpenRouter**:
- Go to https://openrouter.ai/keys
- Copy your API key
- Keep safe: You'll add it to n8n

**Blotato**:
- Go to https://www.blotato.ai/dashboard
- Copy your API key
- Keep safe: You'll add it to n8n

### Step 3: Import n8n Workflow

1. Log into n8n Cloud
2. Click **Workflows** → **Import**
3. Paste the JSON from `n8n-workflow.json` (in this repo)
4. Click **Import**

### Step 4: Configure Credentials

In the imported workflow, update these nodes:

**Node: AI Script Generator**
- Find the header: `Authorization: Bearer YOUR_OPENROUTER_API_KEY`
- Replace with your actual OpenRouter key

**Node: Create Video via Blotato**
- Find the header: `Authorization: Bearer YOUR_BLOTATO_API_KEY`
- Replace with your actual Blotato key

**Node: YouTube Shorts Upload**
- Click "Create Credential"
- Sign in with your Google account
- Authorize YouTube access

**Node: LinkedIn Video Post**
- Click "Create Credential"
- Sign in with your LinkedIn account
- Authorize posting access

### Step 5: Test!

1. Open the workflow
2. Edit "Set Topic" node → change topic to your first SQL/ETL topic
3. Click **Execute Workflow**
4. Watch logs as:
   - Script is generated ✓
   - Video is created ✓
   - Video is downloaded ✓
   - Posted to YouTube ✓
   - Posted to LinkedIn ✓

---

## 📝 Example Topics

```
Suitable for this agent:
- INNER vs LEFT JOIN in SQL
- SQL Window Functions Explained
- Common Table Expressions (CTEs) Tutorial
- Data Normalization Best Practices
- ETL Pipeline Design Patterns
- Handling NULL Values in SQL
- Query Optimization Tips
- Dimensional Modeling Basics
```

---

## 🔄 Workflow Architecture

```
Manual Trigger / Webhook / Cron
    ↓
[Set Topic]
    ↓
[Prepare Prompt]
    ↓
[AI Script Generator] → Returns script + captions
    ↓
[Extract JSON]
    ↓
[Create Video via Blotato] → Returns job_id
    ↓
[Wait for Render] → Polls every 30s until complete
    ↓
[Download Video]
    ↓
┌─→ [YouTube Shorts Upload]
└─→ [LinkedIn Video Post]
```

---

## 📁 Files in This Repo

- **README.md** - This file
- **n8n-workflow.json** - Complete n8n workflow (import this!)
- **prompts.md** - Example AI prompts for different content types
- **setup-guide.md** - Detailed setup troubleshooting
- **.env.example** - Environment variables template (for self-hosted)

---

## ⚡ Quick Start

```bash
# 1. Create n8n account
https://app.n8n.cloud → Sign up

# 2. Get API keys (5 minutes)
OpenRouter: https://openrouter.ai/keys
Blotato: https://www.blotato.ai/dashboard

# 3. Import workflow
Copy content from n8n-workflow.json
Paste into n8n Workflows → Import

# 4. Add credentials
- OpenRouter API Key
- Blotato API Key  
- YouTube OAuth
- LinkedIn OAuth

# 5. Test
Set a topic → Click Execute → Check YouTube + LinkedIn after 5 min
```

---

## 🎬 Video Output Specs

- **Duration**: 60 seconds
- **Aspect Ratio**: 9:16 (vertical for Shorts)
- **Format**: MP4 H.264
- **Voice**: Professional male narrator
- **Style**: Clean, minimal, SQL/code-focused
- **Subtitles**: Auto-generated with code snippets highlighted

---

## 💰 Cost Estimate (Monthly)

| Service | Free Tier | Cost |
|---------|-----------|------|
| n8n Cloud | 5,000 executions | Free* |
| OpenRouter | API usage | ~$5-10 per 100K tokens |
| Blotato | 5-10 free credits | Then $0.5-1 per video |
| YouTube API | Unlimited | Free |
| LinkedIn API | Limited free | Free |
| **Total** | | **$5-15/month** |

*Scales with execution count; 100 videos/month = ~$10/month

---

## 🚀 Advanced: Automate with Cron

Instead of manual triggering, run daily at 7 PM IST:

1. Replace "Manual Trigger" with **Cron node**
2. Set schedule: `0 19 * * *` (7 PM every day)
3. Add a **Google Sheets** node to fetch next topic from a list
4. Deploy workflow

**Example topics list** (update daily):
```
Day 1: INNER vs LEFT JOIN
Day 2: Window Functions
Day 3: CTEs Explained
...
```

---

## 🐛 Troubleshooting

**Problem**: Video API returns error  
**Solution**: Check Blotato credits → https://www.blotato.ai/dashboard

**Problem**: YouTube upload fails  
**Solution**: Verify Google OAuth has YouTube scope enabled

**Problem**: Script generation is slow  
**Solution**: OpenRouter may be processing heavy requests → wait 1 min and retry

**Problem**: LinkedIn post missing format  
**Solution**: Hashtags might exceed LinkedIn text limits; edit the AI prompt to shorten

---

## 📧 Support

For issues:
1. Check `setup-guide.md`
2. Review n8n error logs (Workflow → Executions)
3. Test API keys individually
4. Verify OAuth scopes in developer dashboards

---

## 📄 License

MIT License - Feel free to fork and adapt for your content!

---

## 🎓 What You're Learning

✅ Workflow automation (n8n)  
✅ API integration (OpenRouter, Blotato, YouTube, LinkedIn)  
✅ OAuth authentication  
✅ CI/CD pipeline concepts  
✅ Content automation  
✅ SQL/ETL teaching skills  

Perfect for building your portfolio as an **ETL/Data Engineer** + **Content Creator**!
