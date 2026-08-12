# 🚀 DevSynt AutoGram Engine n8n

## Automated Social Content Pipeline — Google Drive → Supabase → AI Captioning → Instagram

### Made by: Muhammad Talha Zaheer

---

## 📌 Project Overview

The **DevSynt AutoGram Engine** is a fully automated n8n workflow that transforms a Google Drive folder into a self-sustaining Instagram posting machine. It automatically fetches images, generates AI-powered captions using GPT-4 Vision, publishes to Instagram, and maintains comprehensive audit logs in Supabase - all with zero manual intervention.


**Developer:** Muhammad Talha Zaheer

---

## ✨ Key Features

- ✅ **Automated Image Fetching** - Monitors Google Drive folder for new images
- ✅ **Smart Deduplication** - Prevents reposting using Supabase tracking
- ✅ **AI-Powered Captions** - Generates engaging captions and 20 hashtags using GPT-4 Vision
- ✅ **Direct Instagram Publishing** - Posts via Instagram Graph API (2-step process)
- ✅ **Comprehensive Logging** - All posts tracked in Supabase database
- ✅ **Error Handling** - Automatic error logging and email alert system
- ✅ **Zero Human Intervention** - Runs on schedule 24/7

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEVSYNT AUTORAM ENGINE                       │
│                    by Muhammad Talha Zaheer                     │
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

### 2. Supabase API

1. Create project at [Supabase](https://supabase.com)
2. Go to **Project Settings** → **API**
3. Copy **Project URL** (format: `https://your-project.supabase.co`)
4. Copy **Service Role Key** (keep this secure!)
5. Create Storage bucket named `social-images`
6. Run the schema SQL from `database/schema.sql`

### 3. OpenAI API

1. Go to [OpenAI Platform](https://platform.openai.com)
2. Navigate to **API Keys** → **Create new secret key**
3. Copy the API key

### 4. Instagram Graph API

1. Go to [Facebook Developers](https://developers.facebook.com)
2. Create a new app or select existing
3. Add **Instagram Basic Display** product
4. Get Instagram Business Account ID
5. Generate long-lived access token (60+ days)
6. Ensure `instagram_content_publish` permission is granted

---

## 🚀 Quick Installation

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

Open Supabase SQL Editor and run `database/schema.sql`

### Step 4: Import Workflow in n8n

1. Open your n8n instance (http://localhost:5678)
2. Click **Workflows** → **Import from File**
3. Select `workflow.json`
4. Click **Import**

### Step 5: Configure Credentials in n8n

Create these credentials:
- `Google Drive OAuth2` - Client ID & Secret
- `Supabase API` - Project URL & Service Role Key
- `OpenAI API` - API Key
- `Facebook Graph API` - Access Token

### Step 6: Activate Workflow

Toggle workflow to **Active** - it will run every 30 minutes

---

## 📊 Database Schema

**Main tracking table:**

```sql
CREATE TABLE posted_images (
    id BIGINT GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    file_id TEXT UNIQUE NOT NULL,
    filename TEXT NOT NULL,
    image_url TEXT,
    caption TEXT,
    hashtags TEXT[],
    ig_post_id TEXT,
    posted_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

**Error logs table:**

```sql
CREATE TABLE error_logs (
    id BIGINT GENERATED BY DEFAULT AS IDENTITY PRIMARY KEY,
    file_id TEXT,
    error_stage TEXT,
    error_message TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

---

## 🎯 Testing the Workflow

1. Upload a test image to your Google Drive folder
2. In n8n, click **Execute Workflow**
3. Monitor execution in **Executions** tab
4. Check Instagram for the posted image
5. Verify record in Supabase:

```sql
SELECT * FROM posted_images ORDER BY posted_at DESC LIMIT 5;
```

---

## 🔧 Troubleshooting

### Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Google Drive auth fails | Reconnect OAuth2 credentials in n8n |
| Instagram token invalid | Generate new long-lived access token |
| AI caption fails | Check OpenAI API key and balance |
| Duplicate not detected | Verify IF node condition matches results |

### Debugging Tips

**Enable detailed logs:**

```javascript
console.log('Debug:', $json);
```

**Test individual nodes:**
- Right-click on any node → "Execute Node"

**Check Supabase data:**

```sql
SELECT * FROM posted_images ORDER BY posted_at DESC LIMIT 10;
```

**Test Instagram API directly:**

```bash
curl -X GET "https://graph.facebook.com/v21.0/me?access_token=YOUR_TOKEN"
```

---

## ✅ Success Criteria Checklist

- [ ] Workflow runs on schedule without manual trigger
- [ ] Images fetched from Google Drive folder
- [ ] Duplicates prevented (tested with multiple runs)
- [ ] AI generates engaging captions with hashtags
- [ ] Posts published to Instagram successfully
- [ ] All posts logged in Supabase
- [ ] Errors captured and logged properly
- [ ] Documentation complete and tested

---

## 📝 Credential Placeholder Names

| Service | Credential Name in n8n | Fields Required |
|---------|----------------------|-----------------|
| Google Drive | `Google Drive OAuth2` | Client ID, Client Secret |
| Supabase | `Supabase API` | Project URL, Service Role Key |
| OpenAI | `OpenAI API` | API Key |
| Instagram/Facebook | `Facebook Graph API` | Access Token |

---

## 📁 Repository Structure

```
devsynt-autogram-engine/
├── README.md                 # This file
├── workflow.json            # Exported n8n workflow
├── .env.example             # Environment template
├── database/
│   └── schema.sql           # Supabase database schema
├── docs/
│   └── troubleshooting.md   # Troubleshooting guide
└── screenshots/
    ├── workflow_diagram.png # Workflow flow chart
    └── sample_post.png      # Example Instagram post
```

---

## 📄 License

This project is proprietary to DevSynt. All rights reserved.

---

## 🙏 Acknowledgments

- n8n for the amazing automation platform
- Supabase for database and storage
- OpenAI for GPT-4 Vision
- Meta for Instagram Graph API
- Project Lead Usama Shahid for guidance

---

## 🎓 Learning Outcomes

By completing this project, I demonstrated:

1. **Technical Skills**
   - Advanced n8n workflow orchestration
   - REST API integration (Google Drive, Supabase, Instagram)
   - AI/LLM integration with vision capabilities
   - Database design and optimization
   - Error handling and logging

2. **Professional Skills**
   - Production-grade automation engineering
   - Security best practices
   - Documentation standards
   - Testing and quality assurance

3. **Business Value**
   - Zero-touch automation
   - Cost efficiency
   - Scalable architecture
   - Audit compliance

---

**🚀 Built with ❤️ by Muhammad Talha Zaheer**

---

*Version 1.0 | Last Updated: 2026 | DevSynt Technologies*

---

## How to Use This README

1. **Replace `yourusername`** in the clone URL with your actual GitHub username
2. **Add your screenshots** in the `screenshots/` folder
3. **Fill in `.env`** with your actual credentials (never commit this file)
4. **Share the repository link** in the WhatsApp community

---
