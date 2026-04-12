# 🚀 DEPLOYMENT GUIDE - AI YouTube Summarizer

## Architecture Overview
- **Frontend**: React app deployed on Vercel
- **Backend**: Flask API deployed separately (Render/Railway/Heroku)

---

## PART 1: DEPLOY BACKEND (Choose ONE platform)

### Option A: Deploy Backend on Render.com (RECOMMENDED - FREE)

#### Step 1: Prepare Backend for Render
1. Your backend already has `requirements.txt` ✅
2. Make sure `main.py` is in the backend folder ✅

#### Step 2: Deploy on Render
1. Go to https://render.com and sign up/login
2. Click "New +" → "Web Service"
3. Connect your GitHub repository
4. Configure:
   - **Name**: ai-summarizer-backend
   - **Region**: Choose closest to you
   - **Branch**: main
   - **Root Directory**: `AIsummariser_project/backend`
   - **Runtime**: Python 3
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `python main.py`
   - **Plan**: Free

5. Add Environment Variables in Render Dashboard:
   ```
   GEMINI_API_KEY=your_gemini_key_here
   GROQ_API_KEY=your_groq_key_here
   ```

6. Click "Create Web Service"
7. Wait for deployment (2-5 minutes)
8. Copy your backend URL (e.g., `https://ai-summarizer-backend.onrender.com`)

---

### Option B: Deploy Backend on Railway.app (FREE)

1. Go to https://railway.app
2. Click "New Project" → "Deploy from GitHub repo"
3. Select your repository
4. Configure root directory: `AIsummariser_project/backend`
5. Add environment variables (same as Render)
6. Deploy!

---

## PART 2: DEPLOY FRONTEND ON VERCEL

### Step 1: Prepare Frontend
Your frontend is already configured with:
- ✅ `vercel.json` configuration
- ✅ Environment variable support for backend URL
- ✅ `.gitignore` protecting sensitive files

### Step 2: Commit Changes to GitHub
```powershell
cd "c:\Users\AKHIL GOPISHETTI\Downloads\AIsummariser_project\AIsummariser_project"
git add .
git commit -m "Add Vercel deployment configuration"
git push origin main
```

### Step 3: Deploy on Vercel

#### Method A: Using Vercel CLI (Easiest)
```powershell
# Install Vercel CLI
npm install -g vercel

# Navigate to frontend folder
cd frontend

# Login to Vercel
vercel login

# Deploy
vercel --prod
```

#### Method B: Using Vercel Dashboard (Recommended)
1. Go to https://vercel.com and sign up/login with GitHub
2. Click "Add New..." → "Project"
3. Import your GitHub repository
4. Configure:
   - **Framework Preset**: Create React App
   - **Root Directory**: `AIsummariser_project/frontend`
   - **Build Command**: `npm run build` (auto-detected)
   - **Output Directory**: `build` (auto-detected)

5. **IMPORTANT: Add Environment Variable**:
   - Click "Environment Variables"
   - Add: `REACT_APP_BACKEND_URL`
   - Value: Your deployed backend URL (from Part 1)
   - Example: `https://ai-summarizer-backend.onrender.com`
   - Click "Add"

6. Click "Deploy"
7. Wait for deployment (1-2 minutes)
8. Your app is live! 🎉

---

## PART 3: UPDATE CORS SETTINGS (IMPORTANT!)

After deploying backend, update CORS to allow your Vercel domain:

### Edit `backend/main.py`:
```python
# Replace this line:
CORS(app)

# With this:
CORS(app, origins=[
    "http://localhost:3000",  # Local development
    "https://your-app.vercel.app",  # Your Vercel deployment
    "https://*.vercel.app"  # All Vercel preview deployments
])
```

Then commit and redeploy backend:
```powershell
git add backend/main.py
git commit -m "Update CORS for Vercel deployment"
git push origin main
```

---

## PART 4: TESTING YOUR DEPLOYED APP

### Test Local Development:
1. Create `frontend/.env` file:
   ```
   REACT_APP_BACKEND_URL=http://localhost:5000
   ```
2. Run backend: `cd backend && .\venv\Scripts\Activate.ps1 && python main.py`
3. Run frontend: `cd frontend && npm start`
4. Open http://localhost:3000

### Test Production:
1. Open your Vercel URL (e.g., `https://ai-summarizer.vercel.app`)
2. Paste a YouTube URL
3. Click Analyze
4. Verify it connects to your deployed backend

---

## TROUBLESHOOTING

### Issue: CORS Error
**Solution**: Update backend CORS settings (see Part 3)

### Issue: Backend Not Responding
**Solutions**:
- Check Render/Railway logs for errors
- Verify environment variables are set
- Ensure backend URL is correct in Vercel env vars

### Issue: Build Fails on Vercel
**Solutions**:
- Check `vercel.json` is in frontend folder
- Verify `package.json` has correct build script
- Check Vercel build logs for errors

### Issue: API Keys Not Working
**Solutions**:
- Rotate API keys if exposed
- Verify keys are set in backend environment variables
- Check backend logs for authentication errors

---

## ENVIRONMENT VARIABLES SUMMARY

### Backend (Render/Railway):
```
GEMINI_API_KEY=your_key_here
GROQ_API_KEY=your_key_here
```

### Frontend (Vercel):
```
REACT_APP_BACKEND_URL=https://your-backend.onrender.com
```

---

## DEPLOYMENT CHECKLIST

- [ ] Backend deployed on Render/Railway
- [ ] Backend environment variables set
- [ ] CORS updated in backend
- [ ] Frontend committed to GitHub
- [ ] Frontend deployed on Vercel
- [ ] Frontend environment variable set (REACT_APP_BACKEND_URL)
- [ ] Tested local development
- [ ] Tested production deployment
- [ ] Verified API keys are secure (not in code)

---

## URLs

- **GitHub Repo**: https://github.com/anilgopishetti/Ai-Summarizer
- **Vercel Dashboard**: https://vercel.com/dashboard
- **Render Dashboard**: https://dashboard.render.com
- **Railway Dashboard**: https://railway.app/dashboard

---

## NEED HELP?

Common issues and solutions are in the Troubleshooting section above.
For specific errors, check:
- Vercel deployment logs
- Render/Railway service logs
- Browser console for frontend errors
- Network tab for API call details
