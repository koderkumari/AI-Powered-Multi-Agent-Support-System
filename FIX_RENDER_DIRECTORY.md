# Fix for Render Directory Error

## Problem
```
Root directory 'src/frontend/frontend' does not exist, please check settings
```

This error occurs because Render's dashboard settings have an incorrect root directory configuration that conflicts with the `render.yaml`.

## Solution

### Step 1: Update Render Dashboard for Frontend Service

1. Go to Render Dashboard → `ai-support-frontend` service
2. Click **Settings** (top right)
3. Scroll down to **Build & Deploy** section
4. Look for **Root Directory** field
5. **Change from:** `src/frontend/frontend` 
6. **Change to:** `.` (just a dot - means repository root)
7. Click **Save**

The build and start commands in `render.yaml` will handle the `cd` to the correct directory.

### Step 2: Update Render Dashboard for Backend Service

1. Go to Render Dashboard → `ai-support-backend` service
2. Click **Settings**
3. Look for **Root Directory** field
4. **Change from:** whatever it's set to
5. **Change to:** `.` (just a dot - means repository root)
6. Click **Save**

### Step 3: Trigger a Redeploy

Once both services have Root Directory set to `.`:

1. Go to each service
2. Click **Manual Deploy** → **Deploy latest commit**
3. Wait for builds to complete

Both services should now:
- Clone from repository root
- Use `render.yaml` commands to cd into correct directories
- Build and start successfully

## Alternative: Manual Service Configuration

If the above doesn't work, manually configure each service:

### Frontend Service
- **Name:** `ai-support-frontend`
- **Runtime:** Node
- **Root Directory:** `.`
- **Build Command:** `cd frontend/frontend && npm install && npm run build`
- **Start Command:** `cd frontend/frontend && npm start`
- **Environment Variables:**
  - `NODE_ENV=production`
  - `NEXT_PUBLIC_API_URL=https://ai-support-backend.onrender.com`

### Backend Service
- **Name:** `ai-support-backend`
- **Runtime:** Node
- **Root Directory:** `.`
- **Build Command:** `cd frontend/backend && npm install && npm run build`
- **Start Command:** `cd frontend/backend && npm start`
- **Environment Variables:**
  - `NODE_ENV=production`
  - `DATABASE_URL=<your_db_url>`
  - `GROQ_API_KEY=<your_api_key>`

## Why This Works

- By setting Root Directory to `.` (repository root), Render checks out the entire repo
- The `render.yaml` defines commands that navigate to the correct subdirectories
- Each service builds and runs from its own directory
- This approach works for monorepo structures like ours

## Verification

After redeployment:

1. Check frontend service logs - should show: `Server running on http://0.0.0.0:PORT`
2. Check backend service logs - should show: `Server is running on port PORT`
3. Visit: https://ai-powered-multi-agent-support-system.onrender.com
4. Should load without 502 error
