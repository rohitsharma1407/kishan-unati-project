# Kisan Unnati - Deployment Guide

## 🚀 Deployment Options

### Option 1: Vercel + Railway + MongoDB Atlas (Recommended)
### Option 2: Heroku (All-in-one)
### Option 3: DigitalOcean App Platform
### Option 4: AWS (Advanced)

---

## 📋 Prerequisites

Before deployment, ensure you have:

1. **GitHub Repository**: https://github.com/rohitsharma1407/kishan-unati-project
2. **MongoDB Atlas Account**: https://cloud.mongodb.com
3. **Deployment Platform Account**: Vercel/Railway/Heroku/etc.

---

## 🎯 Option 1: Vercel + Railway + MongoDB Atlas (Easiest)

### Step 1: Set up MongoDB Atlas
1. Go to https://cloud.mongodb.com
2. Create account → Create new project → Create cluster
3. Choose "FREE" tier → Create cluster
4. Go to "Network Access" → Add IP Address: `0.0.0.0/0`
5. Go to "Database Access" → Create user with password
6. Go to "Clusters" → Connect → Connect your application
7. Copy the connection string

### Step 2: Deploy Backend to Railway
1. Go to https://railway.app
2. Sign up/Login → Create new project
3. Choose "Deploy from GitHub repo"
4. Connect GitHub → Select `kishan-unati-project`
5. Railway will detect it's a Node.js app
6. Add environment variables:
   ```
   NODE_ENV=production
   PORT=3001
   MONGODB_URI=your_mongodb_atlas_connection_string
   JWT_SECRET=your_super_secret_jwt_key
   FRONTEND_URL=https://your-frontend-domain.vercel.app
   ```
7. Deploy → Get the backend URL (e.g., `https://kisan-backend.railway.app`)

### Step 3: Deploy Frontend to Vercel
1. Go to https://vercel.com
2. Sign up/Login → Import project
3. Connect GitHub → Select `kishan-unati-project`
4. Configure project:
   - **Framework Preset**: Next.js
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `.next`
5. Add environment variables:
   ```
   NEXT_PUBLIC_API_URL=https://your-backend-url.railway.app
   ```
6. Deploy → Get the frontend URL

### Step 4: Update Backend CORS
Update your backend's CORS configuration to allow the Vercel domain.

---

## 🟢 Option 2: Heroku (All-in-one)

### Deploy Backend
1. Install Heroku CLI: https://devcenter.heroku.com/articles/heroku-cli
2. Login: `heroku login`
3. Create app: `heroku create kisan-backend`
4. Set environment variables:
   ```bash
   heroku config:set NODE_ENV=production
   heroku config:set MONGODB_URI=your_mongodb_connection
   heroku config:set JWT_SECRET=your_secret_key
   ```
5. Deploy: `git push heroku main`

### Deploy Frontend
1. In frontend directory, create `vercel.json`:
   ```json
   {
     "version": 2,
     "builds": [
       {
         "src": "package.json",
         "use": "@vercel/next"
       }
     ],
     "routes": [
       {
         "src": "/(.*)",
         "dest": "/$1"
       }
     ],
     "env": {
       "NEXT_PUBLIC_API_URL": "https://your-heroku-backend.herokuapp.com"
     }
   }
   ```
2. Deploy to Vercel as above.

---

## 🔵 Option 3: DigitalOcean App Platform

### Step 1: Create App Spec
Create `backend/.do/app.yaml`:
```yaml
name: kisan-backend
services:
- name: backend
  source_dir: backend
  github:
    repo: rohitsharma1407/kishan-unati-project
    branch: main
  run_command: npm start
  environment_slug: node-js
  instance_count: 1
  instance_size_slug: basic-xxs
  envs:
  - key: NODE_ENV
    value: production
  - key: MONGODB_URI
    value: ${MONGODB_URI}
  - key: JWT_SECRET
    value: ${JWT_SECRET}
```

### Step 2: Deploy
1. Go to https://cloud.digitalocean.com/apps
2. Create app from GitHub repo
3. Use the app spec file
4. Add environment variables
5. Deploy

---

## 🛠️ Environment Variables Required

### Backend (.env)
```env
NODE_ENV=production
PORT=3001
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/kisan_unnati
JWT_SECRET=your_super_secret_jwt_key_here
FRONTEND_URL=https://your-frontend-domain.com
OPENAI_API_KEY=your_openai_key (optional)
WEATHER_API_KEY=your_weather_api_key (optional)
```

### Frontend (.env.local)
```env
NEXT_PUBLIC_API_URL=https://your-backend-domain.com
```

---

## 🔧 Post-Deployment Checklist

- [ ] Test user registration/login
- [ ] Test admin dashboard access
- [ ] Verify database connections
- [ ] Check API endpoints
- [ ] Test file uploads (if any)
- [ ] Verify CORS settings
- [ ] Check environment variables
- [ ] Test AI services (if deployed separately)

---

## 🚀 Production Optimizations

1. **Enable HTTPS**: All deployment platforms provide SSL certificates
2. **Database Indexing**: Ensure MongoDB indexes are created
3. **Caching**: Implement Redis for session/cache (optional)
4. **Monitoring**: Set up error tracking (Sentry, etc.)
5. **Backup**: Configure database backups
6. **CDN**: Use CDN for static assets
7. **Rate Limiting**: Implement API rate limiting

---

## 🐛 Troubleshooting

### Common Issues:
1. **CORS Errors**: Update allowed origins in backend
2. **Database Connection**: Check MongoDB Atlas IP whitelist
3. **Environment Variables**: Ensure all required vars are set
4. **Build Failures**: Check build logs for errors
5. **API Timeouts**: Increase timeout limits if needed

### Debug Commands:
```bash
# Check backend logs
heroku logs --tail -a your-app-name

# Check Vercel deployment
vercel logs your-deployment-url

# Test API endpoints
curl https://your-backend-url/api/auth/test
```

---

## 📞 Support

For deployment issues:
1. Check platform-specific documentation
2. Review build/deployment logs
3. Verify environment configuration
4. Test locally before deploying

Happy deploying! 🚀