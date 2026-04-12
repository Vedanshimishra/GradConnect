# GradConnect - Quick Start Guide

## Run Locally (5 minutes)

### Terminal 1 - Start Backend:
```bash
cd GradConnect-backend
npm install
npm run dev
```
💚 Backend running: http://localhost:5000

### Terminal 2 - Start Frontend:
```bash
cd GradConnect-frontend/alumni-network-glow-1
npm install
npm run dev
```
💙 Frontend running: http://localhost:5173

## Create Test Account:
1. Click "Sign up" on Auth page
2. Email: test@gradconnect.com
3. Password: Test123!
4. Name: Your Name
5. Select: Alumni or Student

## API Endpoints Available:
- `POST /api/auth/register` - Create account
- `POST /api/auth/login` - Login
- More endpoints coming soon!

## Environment Files:
- Backend: `GradConnect-backend/.env`
- Frontend: `GradConnect-frontend/alumni-network-glow-1/.env.local`

## MongoDB Required:
1. Sign up at https://mongodb.com/cloud/atlas
2. Create a free cluster
3. Get connection string
4. Add to `GradConnect-backend/.env` as `MONGO_URI`

## Deploy to Production:
See `DEPLOYMENT_GUIDE.md` for detailed instructions for:
- Render.com (easiest)
- Vercel + Railway
- Custom VPS

## Troubleshooting:
- Backend not starting? Check `.env` file and MongoDB connection
- Frontend not loading? Check `http://localhost:5173` and browser console
- Can't register? Check MongoDB Atlas whitelist IP
