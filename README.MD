# 🚀 DevSynt AutoGram Engine

## Automated Social Media Content Pipeline

---

## 📌 Project Overview

The **DevSynt AutoGram Engine** is a fully automated n8n workflow that transforms a Google Drive folder into a self-sustaining Instagram posting machine. It automatically fetches images, generates AI-powered captions, publishes to Instagram, and maintains comprehensive audit logs - all with zero manual intervention.


---

## ✨ Key Features

- ✅ **Automated Image Fetching** - Monitors Google Drive folder for new images
- ✅ **Smart Deduplication** - Prevents reposting using Supabase tracking
- ✅ **AI-Powered Captions** - Generates engaging captions and hashtags using GPT-4 Vision
- ✅ **Direct Instagram Publishing** - Posts via Instagram Graph API
- ✅ **Comprehensive Logging** - All posts tracked in Supabase database
- ✅ **Error Handling** - Automatic error logging and alert system
- ✅ **Zero Human Intervention** - Runs on schedule 24/7

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEVSYNT AUTORAM ENGINE                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  [Schedule Trigger] ──► Every 30 minutes                       │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ GOOGLE DRIVE INTEGRATION                                │  │
│  │ • OAuth2 Authentication                                │  │
│  │ • Fetch new images from specific folder               │  │
│  │ • Filter: Only image files (jpg, png, gif)            │  │
│  └─────────────────────────────────────────────────────────┘  │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ DEDUPLICATION ENGINE                                    │  │
│  │ • Query Supabase: Check if file_id exists             │  │
│  │ • IF-ELSE: Process new files, skip duplicates          │  │
│  └─────────────────────────────────────────────────────────┘  │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ FILE PROCESSING PIPELINE                               │  │
│  │ • Download image from Google Drive                    │  │
│  │ • Upload to Supabase Storage                          │  │
│  │ • Generate public URL                                 │  │
│  └─────────────────────────────────────────────────────────┘  │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ AI CAPTION GENERATOR                                   │  │
│  │ • GPT-4 Vision image analysis                         │  │
│  │ • Generate engaging captions (120-180 chars)          │  │
│  │ • Auto-hashtag generation (20 hashtags)               │  │
│  └─────────────────────────────────────────────────────────┘  │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ SOCIAL PUBLISHING ENGINE                               │  │
│  │ • Create media container                              │  │
│  │ • Publish to Instagram Business Account               │  │
│  └─────────────────────────────────────────────────────────┘  │
│         │                                                       │
│         ▼                                                       │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │ LOGGING SYSTEM                                         │  │
│  │ • Insert record into Supabase tracking table          │  │
│  │ • Store IG post ID, caption, hashtags                 │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                                  │
│  [Error Handler] ──► Supabase Error Logs ──► Email Alerts    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| Automation Engine | n8n (self-hosted) | Workflow orchestration |
| File Source | Google Drive API v3 | Image repository |
| Database & Storage | Supabase (PostgreSQL + Storage) | Tracking, logging & image hosting |
| AI Captioning | OpenAI GPT-4 Vision | Caption & hashtag generation |
| Social Platform | Instagram Graph API v21.0 | Content publishing |

---

## 📋 Prerequisites

Before setting up this workflow, ensure you have:

1. **n8n Instance** - Self-hosted or n8n cloud
2. **Google Cloud Project** - With Drive API enabled
3. **Supabase Project** - With PostgreSQL database
4. **OpenAI Account** - With API access (GPT-4 Vision)
5. **Facebook Developer Account** - With Instagram Graph API access
6. **Instagram Business Account** - Connected to Facebook Page

---

## 🔐 Credential Setup Guide

### 1. Google Drive OAuth2

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select existing
3. Enable **Google Drive API**
4. Navigate to **Credentials** → **Create Credentials** → **OAuth Client ID**
5. Select **Web application** as application type
6. Add redirect URI: `https://your-n8n-domain.com/oauth2-credential/callback`
7. Copy **Client ID** and **Client Secret**
8. In n8n, create credential with name: `Google Drive OAuth2`

### 2. Supabase API

1. Create project at [Supabase](https://supabase.com)
2. Go to **Project Settings** → **API**
3. Copy **Project URL** (format: `https://your-project.supabase.co`)
4. Copy **Service Role Key** (keep this secure!)
5. Create Storage bucket named `social-images`
6. Run the schema SQL from `database/schema.sql`
7. In n8n, create credential with name: `Supabase API`

### 3. OpenAI API

1. Go to [OpenAI Platform](https://platform.openai.com)
2. Navigate to **API Keys** → **Create new secret key**
3. Copy the API key
4. In n8n, create credential with name: `OpenAI API`

### 4. Instagram Graph API

1. Go to [Facebook Developers](https://developers.facebook.com)
2. Create a new app or select existing
3. Add **Instagram Basic Display** product
4. Get Instagram Business Account ID:
   - Go to Facebook Page → About → Page ID
   - Use Graph API Explorer to get Instagram ID
5. Generate long-lived access token (60+ days):
   ```bash
   # Exchange short-lived token for long-lived
   GET https://graph.facebook.com/v21.0/oauth/access_token?
     grant_type=fb_exchange_token&
     client_id={app-id}&
     client_secret={app-secret}&
     fb_exchange_token={short-lived-token}
   ```
6. Ensure `instagram_content_publish` permission is granted
7. In n8n, create credential with name: `Facebook Graph API`

---

## 🚀 Installation & Setup

### Step 1: Clone Repository

```bash
git clone https://github.com/yourusername/devsynt-autogram-engine.git
cd devsynt-autogram-engine
```

### Step 2: Configure Environment

```bash
cp .env.example .env
# Edit .env with your actual credentials
```

### Step 3: Database Setup

Open Supabase SQL Editor and run:

```sql
-- Execute database/schema.sql
-- Creates posted_images and error_logs tables
```

### Step 4: Import Workflow in n8n

1. Open your n8n instance (http://localhost:5678)
2. Click **Workflows** → **Import from File**
3. Select `workflow.json` from this repository
4. Click **Import**

### Step 5: Configure Credentials in n8n

Navigate to **Credentials** and create:

| Credential Name | Type | Required Fields |
|-----------------|------|-----------------|
| Google Drive OAuth2 | OAuth2 | Client ID, Client Secret |
| Supabase API | API Key | Project URL, Service Role Key |
| OpenAI API | API Key | API Key |
| Facebook Graph API | OAuth2 | Access Token |

### Step 6: Configure Google Drive Folder

1. In workflow, find node "Google Drive Search"
2. Replace `YOUR_FOLDER_ID` with your Google Drive folder ID
3. **To get folder ID:** Open folder in Drive, copy from URL

### Step 7: Configure Instagram User ID

1. Find nodes "Create Instagram Container" and "Publish to Instagram"
2. Replace `YOUR_IG_USER_ID` with your Instagram Business Account ID

### Step 8: Activate Workflow

1. Toggle workflow to **Active** (green switch)
2. The workflow will run every 30 minutes automatically
3. Or click **Execute Workflow** for manual test

---

## 📝 Credential Placeholder Names

| Service | Credential Name in n8n | Fields Required |
|---------|----------------------|-----------------|
| Google Drive | `Google Drive OAuth2` | Client ID, Client Secret |
| Supabase | `Supabase API` | Project URL, Service Role Key |
| OpenAI | `OpenAI API` | API Key |
| Instagram/Facebook | `Facebook Graph API` | Access Token |

---

## 🎯 Testing the Workflow

### Test with a Single Image

1. Upload a test image to your Google Drive folder
2. In n8n, click **Execute Workflow** button (top right)
3. Monitor execution in **Executions** tab
4. Check Instagram for the posted image

### Verify Database Records

```sql
-- Check successful posts
SELECT * FROM posted_images ORDER BY posted_at DESC LIMIT 10;

-- Check for errors
SELECT * FROM error_logs ORDER BY created_at DESC LIMIT 10;
```

---

## 📊 Monitoring

### Health Check Queries

**Last 24 hours performance:**

```sql
SELECT 
    COUNT(*) as total_posts,
    COUNT(CASE WHEN status = 'published' THEN 1 END) as successful,
    COUNT(CASE WHEN status = 'failed' THEN 1 END) as failed
FROM posted_images
WHERE posted_at >= NOW() - INTERVAL '24 hours';
```

**Recent errors:**

```sql
SELECT 
    error_stage,
    COUNT(*) as error_count,
    MAX(created_at) as last_occurrence
FROM error_logs
WHERE created_at >= NOW() - INTERVAL '24 hours'
GROUP BY error_stage
ORDER BY error_count DESC;
```

**Daily summary:**

```sql
SELECT * FROM daily_post_summary;
```

---

## 🔧 Troubleshooting

### Common Issues & Solutions

#### Issue 1: Workflow Fails at Google Drive

**Error:** `"Invalid OAuth2 credentials"`

**Solution:**
1. Go to n8n Credentials → Google Drive OAuth2
2. Click "Reconnect" to refresh token
3. Verify redirect URI matches n8n URL

#### Issue 2: Instagram Publishing Fails

**Error:** `"Invalid access token"`

**Solution:**
1. Generate new long-lived token
2. Check permissions in Facebook Developer Dashboard
3. Verify Instagram Account is Business/Creator type

#### Issue 3: Duplicate Posts Not Detected

**Error:** Same image gets posted multiple times

**Solution:**
1. Check if `file_id` is unique in database
2. Verify the IF node condition is correct
3. Clear any duplicate records in database

#### Issue 4: AI Caption Generation Fails

**Error:** `"Insufficient quota"` or `"Model not available"`

**Solution:**
1. Check OpenAI account balance
2. Verify access to GPT-4 Vision
3. Add fallback to GPT-3.5-Turbo

### Debugging Tips

**Enable detailed logs:**

```javascript
// Add to Function nodes
console.log('Debug:', $json);
```

**Test individual nodes:**
- Right-click on any node → "Execute Node"
- Check output data format

---

## 📁 Repository Structure

```
devsynt-autogram-engine/
├── README.md                 # This file
├── workflow.json            # Exported n8n workflow
├── .env.example             # Environment template
├── database/
│   └── schema.sql           # Supabase database schema
├── screenshots/
│   ├── workflow_diagram.png # Workflow flow chart
│   └── sample_post.png      # Example Instagram post
└── docs/
    └── troubleshooting.md   # Detailed troubleshooting guide
```

---

## ✅ Success Checklist

- [ ] Workflow runs on schedule without manual trigger
- [ ] Images fetched from Google Drive folder
- [ ] Duplicates prevented (tested with multiple runs)
- [ ] AI generates engaging captions with hashtags
- [ ] Posts published to Instagram successfully
- [ ] All posts logged in Supabase
- [ ] Errors captured and logged properly
- [ ] Notifications sent for failures
- [ ] Documentation complete and tested
- [ ] Demo screenshot/video included

---

## 📞 Support

- **Project Lead:** Usama Shahid
- **Community:** WhatsApp group
- **Rise Portal:** For official queries
- **Email:** technical@devsynt.com

---

## 📄 License

This project is proprietary to DevSynt. All rights reserved.

---

## 🙏 Acknowledgments

- n8n for the amazing automation platform
- Supabase for database and storage
- OpenAI for GPT-4 Vision
- Meta for Instagram Graph API

---

**🚀 Happy Automating!**

---

*Version 1.0 | Last Updated: 2026 | DevSynt Technologies*