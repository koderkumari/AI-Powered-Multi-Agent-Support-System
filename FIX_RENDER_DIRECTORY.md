# Fix Render Directory Error

## Problem
Render is looking for the frontend at the wrong path: `src/frontend/frontend` instead of `frontend/frontend`

## Solution

### For Frontend Service (ai-support-frontend):

1. Go to Render Dashboard
2. Click on the **ai-support-frontend** service
3. Click **Settings** (top right)
4. Scroll down to **Root Directory**
5. Change from: `src/frontend/frontend` → To: `frontend/frontend`
6. Click **Save**

### For Backend Service (ai-support-backend):

1. Go to Render Dashboard
2. Click on the **ai-support-backend** service
3. Click **Settings** (top right)
4. Scroll down to **Root Directory**
5. Verify it shows: `frontend/backend`
6. If different, change to: `frontend/backend`
7. Click **Save**

### After Updating Settings:

1. Go to the frontend service
2. Click **Manual Deploy** → **Deploy latest commit**
3. Monitor the logs for:
   - ✓ "npm install" completes
   - ✓ "npm run build" completes  
   - ✓ "Server running on http://0.0.0.0:PORT"

## Correct Directory Structure (from repo root):

```
.
├── frontend/
│   ├── backend/          ← Backend service rootDir
│   └── frontend/         ← Frontend service rootDir
├── render.yaml
└── package.json
```

## Environment Variables to Verify

**Frontend (ai-support-frontend):**
- `NODE_ENV` = `production`
- `NEXT_PUBLIC_API_URL` = `https://ai-support-backend.onrender.com`

**Backend (ai-support-backend):**
- `NODE_ENV` = `production`
- `DATABASE_URL` = Your Supabase URL (must be set)
- `GROQ_API_KEY` = Your Groq API key (must be set)

## If Still Having Issues

1. Try deleting both services and recreating them
2. Or disconnect and reconnect the GitHub repo
3. Make sure `render.yaml` is in the repository root
