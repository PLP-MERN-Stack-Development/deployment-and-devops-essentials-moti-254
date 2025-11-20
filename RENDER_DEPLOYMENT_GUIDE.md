# Step 3: Backend Deployment to Render - Complete Guide

## What is Render?
Render is a modern cloud platform that makes deploying applications easy. It automatically deploys from your GitHub repository and handles SSL certificates.

## Prerequisites
✅ GitHub account with your repository pushed
✅ Render account created (https://render.com)
✅ MongoDB Atlas connection string ready
✅ `.env` variables documented

---

## Deployment Steps

### Step 1: Connect Your GitHub Repository to Render

1. **Log in to Render**
   - Go to https://render.com and sign in
   - Click your profile icon → Dashboard

2. **Create New Service**
   - Click **"New +"** button (top right)
   - Select **"Web Service"**

3. **Connect GitHub**
   - Click **"Connect account"** next to GitHub
   - Authorize Render to access your GitHub repositories
   - Select your `deployment-and-devops-essentials-moti-254` repository
   - Click **"Connect"**

4. **Choose Repository Branch**
   - Select branch: **main**
   - Click **"Continue"**

---

### Step 2: Configure Your Service

Fill in the following details:

| Field | Value |
|-------|-------|
| **Name** | `mern-chat-backend` (or any unique name) |
| **Environment** | `Node` |
| **Region** | Choose closest to you (e.g., `us-east-1`) |
| **Branch** | `main` |
| **Build Command** | `cd app/server && npm install` |
| **Start Command** | `cd app/server && node server.js` |
| **Plan** | `Free` (for development) |

**Note:** The build/start commands include `cd app/server` because Render deploys from the repo root.

---

### Step 3: Add Environment Variables

1. Scroll down to **"Environment Variables"** section
2. Click **"Add Environment Variable"** for each variable:

| Key | Value | Notes |
|-----|-------|-------|
| `NODE_ENV` | `production` | Required |
| `PORT` | `5000` | Render provides PORT, but explicit is good |
| `CLIENT_URL` | `https://your-frontend-vercel-url.vercel.app` | Update after Vercel deployment |
| `MONGODB_URI` | `mongodb+srv://user:password@cluster.mongodb.net/dbname` | From MongoDB Atlas |
| `JWT_SECRET` | Your strong secret key | Generate a strong random string |
| `LOG_LEVEL` | `info` | For production logging |

**To generate a strong JWT_SECRET:**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

---

### Step 4: Deploy

1. Review all settings one final time
2. Click **"Create Web Service"**
3. Render will start building:
   - Building container (~2-3 minutes)
   - Deploying service
   - Shows live logs

---

### Step 5: Verify Deployment

Once deployment completes:

1. **Check Status**
   - Status should show **"Live"** in green
   - You'll see your service URL: `https://mern-chat-backend-xxxxx.onrender.com`

2. **Test Your API**
   ```bash
   # In PowerShell
   Invoke-WebRequest -Uri "https://mern-chat-backend-xxxxx.onrender.com" -UseBasicParsing
   ```

3. **Expected Response:**
   ```json
   {
     "message": "Socket.io Chat Server is running",
     "version": "1.0.0",
     "endpoints": ["/api/messages", "/api/users", "/api/rooms"]
   }
   ```

---

## Important Notes

### Free Tier Limitations
- Services spin down after 15 minutes of inactivity
- First request will be slow (cold start)
- For production, upgrade to paid plan

### Automatic Redeployment
- Every push to `main` branch triggers automatic deployment
- Check **"Deploy"** tab for deployment logs

### Custom Domain (Optional)
1. Go to service settings
2. Scroll to **"Custom Domain"**
3. Add your domain and follow DNS setup

### Troubleshooting

**Service won't start?**
- Check build logs in Dashboard
- Verify all environment variables are set
- Ensure MongoDB connection string is correct

**Socket.io connection refused?**
- Update `CLIENT_URL` in environment variables
- Clear browser cache
- Check CORS configuration in server

**Too many restarts?**
- Check logs for errors
- Verify PORT is not hardcoded (should use `process.env.PORT`)

---

## Next Step: Update Frontend Configuration

After backend is live:

1. Copy your Render URL: `https://mern-chat-backend-xxxxx.onrender.com`
2. Go to Render Dashboard → Your Backend Service
3. Update `CLIENT_URL` environment variable
4. Redeploy for changes to take effect

---

## Monitoring & Logs

In Render Dashboard:

1. **Logs Tab** - Real-time server logs
2. **Metrics Tab** - CPU, memory, network usage
3. **Events Tab** - Deployment history
4. **Settings Tab** - Update configuration

---

## Git Workflow

Keep deploying changes easily:

```bash
# Make changes locally
# Test locally first

# Commit and push
git add .
git commit -m "Production optimizations"
git push origin main

# Render automatically redeploys!
```
