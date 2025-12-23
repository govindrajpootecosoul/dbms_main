# Vercel Framework Presets Configuration

## Project Structure
```
dbms/
├── my_database_portal_backend/    # Backend API
└── my_database_portal_frontend/   # Frontend React App
```

## Framework Presets

### 1. Frontend Project (`my_database_portal_frontend`)

**Framework Preset:** `Vite`

**Settings:**
- **Root Directory:** `my_database_portal_frontend`
- **Framework Preset:** `Vite`
- **Build Command:** `npm run build`
- **Output Directory:** `dist`
- **Install Command:** `npm install`
- **Node.js Version:** `18.x` or `20.x`

**Environment Variables:**
- `VITE_API_URL` (if you need to point to backend URL)
  - Example: `https://your-backend-project.vercel.app`

---

### 2. Backend Project (`my_database_portal_backend`)

**Framework Preset:** `Other` (or leave blank)

**Settings:**
- **Root Directory:** `my_database_portal_backend`
- **Framework Preset:** `Other` (or `None`)
- **Build Command:** (leave empty)
- **Output Directory:** (leave empty)
- **Install Command:** `npm install`
- **Node.js Version:** `18.x` or `20.x`

**Environment Variables:**
- `JWT_SECRET` (REQUIRED)
  - Generate: `openssl rand -base64 32`
- `MONGODB_URI` (Optional - has default)
  - Your MongoDB connection string

---

## Deployment Steps

### Separate Projects (Recommended)

#### Deploy Frontend:
1. Go to Vercel Dashboard
2. Click "Add New Project"
3. Import your Git repository
4. Configure:
   - **Project Name:** `dbms-portal-frontend` (or your choice)
   - **Root Directory:** `my_database_portal_frontend`
   - **Framework Preset:** `Vite`
   - **Build Command:** `npm run build`
   - **Output Directory:** `dist`
5. Add environment variables if needed
6. Click "Deploy"

#### Deploy Backend:
1. Go to Vercel Dashboard
2. Click "Add New Project"
3. Import the same Git repository
4. Configure:
   - **Project Name:** `dbms-portal-backend` (or your choice)
   - **Root Directory:** `my_database_portal_backend`
   - **Framework Preset:** `Other`
   - **Build Command:** (leave empty)
   - **Output Directory:** (leave empty)
5. **IMPORTANT:** Add environment variables:
   - `JWT_SECRET` (required)
   - `MONGODB_URI` (optional)
6. Click "Deploy"

### Update Frontend API URL

After backend is deployed, update frontend to use backend URL:

1. In Vercel Frontend project settings:
   - Add environment variable: `VITE_API_URL`
   - Value: `https://your-backend-project.vercel.app`
2. Update frontend API configuration to use `import.meta.env.VITE_API_URL`

---

## Quick Reference

| Project | Framework | Root Directory | Build Command | Output |
|---------|-----------|----------------|---------------|--------|
| Frontend | **Vite** | `my_database_portal_frontend` | `npm run build` | `dist` |
| Backend | **Other** | `my_database_portal_backend` | (empty) | (empty) |

---

## Notes

- **Frontend** uses Vite, so Framework Preset should be **Vite**
- **Backend** is a custom Express serverless function, so use **Other** or **None**
- Both projects can be in the same Git repository
- Deploy them as separate Vercel projects for better management
- Always set `JWT_SECRET` in backend project before first deployment

