# Vercel Deployment Guide

## Environment Variables Setup

Before deploying to Vercel, you need to set the following environment variables in your Vercel dashboard:

### Required Environment Variables:

1. **JWT_SECRET** (Required)
   - Generate a strong secret: `openssl rand -base64 32`
   - Add in Vercel: Project Settings > Environment Variables
   - Add for: Production, Preview, and Development environments

2. **MONGODB_URI** (Optional - has default)
   - Your MongoDB connection string
   - Format: `mongodb+srv://username:password@cluster.mongodb.net/dbname?retryWrites=true&w=majority`
   - If not set, will use the default connection string

### Steps to Add Environment Variables in Vercel:

1. Go to your Vercel project dashboard
2. Click on **Settings** > **Environment Variables**
3. Add each variable:
   - **Name**: `JWT_SECRET`
   - **Value**: (paste your generated secret)
   - **Environment**: Select all (Production, Preview, Development)
4. Click **Save**
5. **Important**: Redeploy your project after adding environment variables

## Project Structure for Vercel

```
my_database_portal_backend/
├── api/
│   └── index.js          # Vercel serverless function handler
├── config/
│   └── db.js            # Database connection
├── middleware/
│   └── auth.js          # Authentication middleware
├── models/              # Mongoose models
├── routes/              # Express routes
└── server.js            # Local development server

vercel.json              # Vercel configuration
```

## Local Development

For local development, use:
```bash
cd my_database_portal_backend
npm install
npm run dev
```

Create a `.env` file in `my_database_portal_backend/`:
```
JWT_SECRET=your_local_secret_key_here
MONGODB_URI=your_mongodb_connection_string
```

## Vercel Deployment

1. **Install Vercel CLI** (optional):
   ```bash
   npm i -g vercel
   ```

2. **Deploy**:
   ```bash
   vercel
   ```

   Or push to your Git repository connected to Vercel.

3. **After adding environment variables**, redeploy:
   - Go to Vercel dashboard
   - Click **Deployments** tab
   - Click **Redeploy** on the latest deployment

## Testing the Deployment

After deployment, test your API:
- Health check: `https://your-project.vercel.app/api/health`
- Should return: `{"success": true, "message": "Server is running"}`

## Troubleshooting

### Error: JWT_SECRET not set
- Make sure you added `JWT_SECRET` in Vercel environment variables
- Redeploy after adding environment variables
- Check that it's set for the correct environment (Production/Preview)

### Error: Database connection failed
- Check your `MONGODB_URI` is correct
- Ensure MongoDB Atlas allows connections from Vercel IPs (0.0.0.0/0)
- Check MongoDB connection string format

### Error: 500 Internal Server Error
- Check Vercel function logs in the dashboard
- Verify all environment variables are set
- Ensure database connection is working

## Notes

- The `server.js` file is for local development only
- Vercel uses `api/index.js` as the serverless function entry point
- Database connections are cached for better performance in serverless environment
- Environment variables must be set in Vercel dashboard, not in `.env` files

