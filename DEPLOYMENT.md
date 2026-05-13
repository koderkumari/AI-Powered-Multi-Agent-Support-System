# Deployment Guide for Render

## Issues Fixed

1. **Custom Server Handler** - Created `server.js` to properly handle `PORT` environment variable from Render
2. **Next.js Configuration** - Added `output: 'standalone'` for optimized production build
3. **Frontend First** - Made frontend the primary service for the main domain

## Deployment Checklist

### 1. Backend Service Setup

**Dashboard:**
- Service Name: `ai-support-backend`
- GitHub Branch: `main`
- Root Directory: `frontend/backend`
- Build Command: `npm install && npm run build`
- Start Command: `npm start`

**Environment Variables (add in Render dashboard):**
```
NODE_ENV=production
DATABASE_URL=your_supabase_database_url
GROQ_API_KEY=your_groq_api_key
```

**Verify:**
- ✓ Backend should start on dynamic PORT (e.g., https://ai-support-backend.onrender.com)
- ✓ Check logs for "Server is running on port"

### 2. Frontend Service Setup

**Dashboard:**
- Service Name: `ai-support-frontend`
- GitHub Branch: `main`
- Root Directory: `frontend/frontend`
- Build Command: `npm install && npm run build`
- Start Command: `npm start`

**Environment Variables (add in Render dashboard):**
```
NODE_ENV=production
NEXT_PUBLIC_API_URL=https://ai-support-backend.onrender.com
```

**Verify:**
- ✓ Frontend should be accessible at https://ai-powered-multi-agent-support-system.onrender.com
- ✓ Check logs for "Server running on"

### 3. Troubleshooting 502 Errors

**Check these in order:**

1. **Service Status**
   - Go to Render Dashboard
   - Click each service and check "Logs"
   - Look for any errors during build or startup

2. **Build Logs (in order)**
   ```
   - npm install progress
   - npm run build progress
   - Any TypeScript errors?
   - Any missing dependencies?
   ```

3. **Startup Logs**
   ```
   Frontend should show: "Server running on http://0.0.0.0:XXXX"
   Backend should show: "Server is running on port XXXX"
   ```

4. **Environment Variables**
   - Verify all variables are set correctly in dashboard
   - Check for typos in key names
   - Ensure GROQ_API_KEY and DATABASE_URL are present

5. **Port Configuration**
   - Frontend uses custom `server.js` that reads PORT from env
   - Backend's `index.ts` reads PORT from env
   - Both should start successfully

### 4. If Still Getting 502

**Common Causes:**

1. **Build Failed** → Check full build logs for errors
2. **Dependencies Missing** → Check package.json has all required packages
3. **Database Error** → Verify DATABASE_URL is correct and accessible
4. **PORT Not Being Read** → Verify `server.js` and `index.ts` are using PORT env var

**Debug Steps:**

1. Check frontend logs:
   - Look for ".next directory not found" → build failed
   - Look for "Error:" → runtime error
   
2. Check backend logs:
   - Look for database connection errors
   - Look for missing environment variables

### 5. Verification

**Frontend is working:**
- Can access https://ai-powered-multi-agent-support-system.onrender.com
- Page loads without errors
- Check browser console for any errors

**Backend is working:**
- Can access https://ai-support-backend.onrender.com/api/health
- Should return JSON: `{"status":"ok","timestamp":"...","uptime":...}`

**Communication:**
- Frontend can reach backend API
- Check browser Network tab for API calls
- Verify NEXT_PUBLIC_API_URL matches backend URL

## Quick Re-deployment

If you need to redeploy:

1. Make changes locally
2. Commit and push to GitHub `main` branch
3. Render automatically detects and rebuilds
4. Monitor logs for build completion

## Local Testing

Before deploying, test locally:

```bash
# Install dependencies
npm run install-all

# Run dev servers
npm run dev

# Test frontend: http://localhost:3000
# Test backend: http://localhost:8000/api/health
```

## Support Files

- **render.yaml** - Defines both services
- **server.js** (frontend) - Custom server handler
- **.env.local** - Local development environment
- **next.config.mjs** - Next.js production config
