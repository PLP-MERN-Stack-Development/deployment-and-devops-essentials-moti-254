# Render Deployment Quick Start

## Quick Reference: What You Need

Before starting deployment, gather:
1. Your Render account (https://render.com)
2. Your GitHub repository URL
3. MongoDB Atlas connection string
4. A JWT secret (generate: `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`)

---

## 5-Minute Deployment

### 1. Go to Render Dashboard
- Visit https://render.com/dashboard
- Click **"New +"** → **"Web Service"**

### 2. Connect GitHub (First Time Only)
- Click **"Connect account"** next to GitHub
- Authorize access
- Select: `deployment-and-devops-essentials-moti-254`

### 3. Configure Service

**Basic Settings:**
```
Name: mern-chat-backend
Environment: Node
Region: us-east-1 (or nearest)
Branch: main
Build Command: cd app/server && npm install
Start Command: cd app/server && node server.js
Plan: Free
```

### 4. Add Environment Variables

Click "Advanced" → "Add Environment Variable" for each:

```
NODE_ENV = production
PORT = 5000
MONGODB_URI = mongodb+srv://user:password@cluster0.xxxxx.mongodb.net/dbname
JWT_SECRET = (generate a long random string)
CLIENT_URL = https://your-vercel-url.vercel.app (add later)
LOG_LEVEL = info
```

### 5. Deploy!
- Click **"Create Web Service"**
- Wait for build to complete (2-3 minutes)
- You'll see: **"Live"** ✅

### 6. Test Your Backend
Get your URL from the dashboard (like: `https://mern-chat-backend-abc123.onrender.com`)

Test it:
```powershell
Invoke-WebRequest -Uri "https://your-render-url.onrender.com"
```

Expected response:
```json
{
  "message": "Socket.io Chat Server is running",
  "version": "1.0.0",
  "endpoints": ["/api/messages", "/api/users", "/api/rooms"]
}
```

---

## Common Issues & Fixes

| Issue | Solution |
|-------|----------|
| Build fails | Check MongoDB connection string in env vars |
| Service won't start | Verify PORT and NODE_ENV are set |
| Socket.io won't connect from frontend | Update CLIENT_URL in Render dashboard |
| Slow first request | Free tier has cold starts - upgrade to paid for production |

---

## After Backend is Live

1. Copy your backend URL from Render dashboard
2. Go to next step: **Step 4 - Frontend Deployment to Vercel**
3. Update `.env.production` with backend URL
4. Deploy frontend

---

## Useful Commands

```bash
# View Render logs (in dashboard)
# Real-time logs available in Render → Your Service → Logs

# Manually redeploy (optional)
# In Render dashboard → Deploys → Manual Deploy

# Update environment variables
# Render Dashboard → Service → Environment
```

Ready to deploy? Go to https://render.com/dashboard and follow the "5-Minute Deployment" steps above!
