# Vercel Deployment Setup - Quick Guide

## ✅ What Was Fixed

1. **Created Vercel Serverless Function** (`my_database_portal_backend/api/index.js`)
   - Exports default handler for Vercel
   - Handles Express app in serverless environment
   - Implements database connection caching

2. **Created Vercel Configuration** (`vercel.json`)
   - Routes all requests to the serverless function
   - Configures Node.js runtime

3. **Updated Database Connection** (`my_database_portal_backend/config/db.js`)
   - Won't exit process in serverless environment
   - Better error handling for Vercel

4. **Updated Error Messages**
   - Now mentions Vercel environment variables

## 🔧 Required Steps Before Deployment

### 1. Set Environment Variables in Vercel

Go to your Vercel project dashboard:
1. Click **Settings** > **Environment Variables**
2. Add these variables:

   **JWT_SECRET** (REQUIRED)
   - Generate: `openssl rand -base64 32`
   - Value: (paste generated secret)
   - Environments: Production, Preview, Development

   **MONGODB_URI** (Optional - has default)
   - Your MongoDB connection string
   - Only needed if you want to override the default

3. **IMPORTANT**: After adding variables, click **Redeploy** on your latest deployment

### 2. Deploy to Vercel

```bash
# Option 1: Via Git (Recommended)
git add .
git commit -m "Configure for Vercel deployment"
git push

# Option 2: Via Vercel CLI
vercel
```

### 3. Test Your Deployment

After deployment, test:
- Health check: `https://your-project.vercel.app/api/health`
- Should return: `{"success": true, "message": "Server is running"}`

## 📁 Project Structure

```
dbms/
├── my_database_portal_backend/
│   ├── api/
│   │   └── index.js          # Vercel serverless function
│   ├── config/
│   │   └── db.js            # Database connection
│   ├── routes/              # API routes
│   ├── models/              # Mongoose models
│   └── server.js            # Local dev server (not used by Vercel)
├── my_database_portal_frontend/
│   └── ...                  # Frontend code
└── vercel.json              # Vercel configuration
```

## 🚨 Common Issues & Solutions

### Issue: JWT_SECRET not set error
**Solution**: 
- Add `JWT_SECRET` in Vercel environment variables
- Redeploy after adding

### Issue: 500 Internal Server Error
**Solution**:
- Check Vercel function logs
- Verify all environment variables are set
- Check database connection string

### Issue: Database connection timeout
**Solution**:
- Ensure MongoDB Atlas allows connections from anywhere (0.0.0.0/0)
- Check connection string format

## 📝 Notes

- `server.js` is only for local development
- Vercel uses `api/index.js` as entry point
- Environment variables must be set in Vercel dashboard
- Database connections are cached for performance
- Always redeploy after adding/changing environment variables

