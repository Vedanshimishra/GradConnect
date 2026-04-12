# GradConnect - Deployment & Setup Guide

## Prerequisites
- Node.js (v16 or higher)
- MongoDB Atlas account (https://www.mongodb.com/cloud/atlas)
- Git
- npm or yarn package manager

---

## Part 1: Local Development Setup

### Step 1: Backend Setup

1. Navigate to the backend directory:
   ```bash
   cd GradConnect-backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create/update `.env` file with your MongoDB credentials:
   ```env
   PORT=5000
   MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/gradconnect
   JWT_SECRET=your_secure_jwt_secret_key_here
   NODE_ENV=development
   ```

4. Start the backend server:
   ```bash
   npm run dev
   ```
   The backend will run on `http://localhost:5000`

### Step 2: Frontend Setup

1. Navigate to the frontend directory:
   ```bash
   cd GradConnect-frontend/alumni-network-glow-1
   ```

2. Install dependencies:
   ```bash
   npm install
   # or if using bun:
   bun install
   ```

3. The `.env.local` is already configured for development:
   ```env
   VITE_API_URL=http://localhost:5000/api
   ```

4. Start the development server:
   ```bash
   npm run dev
   ```
   The frontend will run on `http://localhost:5173`

5. Open your browser and navigate to `http://localhost:5173`

---

## Part 2: Production Deployment Options

### Option A: Deploy on Render.com (Recommended for beginners)

#### Backend Deployment:
1. Push your code to a GitHub repository
2. Go to https://render.com
3. Connect your GitHub account
4. Create a new Web Service
5. Select your GradConnect-backend repository
6. Configure:
   - Build Command: `npm install`
   - Start Command: `npm start`
   - Environment Variables:
     ```
     PORT=5000
     MONGO_URI=your_production_mongodb_uri
     JWT_SECRET=your_production_jwt_secret
     NODE_ENV=production
     FRONTEND_URL=https://your-frontend-url.com
     ```
7. Deploy!

#### Frontend Deployment:
1. Create a new Static Site on Render.com
2. Select your GradConnect-frontend repository
3. Configure:
   - Build Command: `npm run build`
   - Publish Directory: `dist`
   - Environment:
     ```
     VITE_API_URL=https://your-backend-url.onrender.com/api
     ```
4. Deploy!

### Option B: Deploy on Vercel (For Frontend) + Railway (For Backend)

#### Backend on Railway:
1. Go to https://railway.app
2. Create new project
3. Connect your GitHub
4. Select backend folder
5. Add environment variables from `.env`
6. Railway will auto-detect Node.js and start the service
7. Note the generated URL

#### Frontend on Vercel:
1. Go to https://vercel.com
2. Import your GitHub repo
3. Select frontend folder
4. Set environment variable:
   ```
   VITE_API_URL=https://your-railway-backend-url/api
   ```
5. Deploy!

### Option C: Manual Deployment (VPS/Cloud Server)

#### Backend:
```bash
# SSH into your server
ssh user@your-server-ip

# Clone repository
git clone <your-repo-url>
cd GradConnect-backend

# Install Node.js (if not installed)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install dependencies
npm install

# Create .env file with production values
nano .env

# Install PM2 for process management
npm install -g pm2

# Start the application
pm2 start server.js --name "gradconnect-backend"
pm2 startup
pm2 save

# Install nginx for reverse proxy
sudo apt install nginx

# Configure nginx (create /etc/nginx/sites-available/gradconnect)
# Add reverse proxy configuration pointing to localhost:5000
```

#### Frontend:
```bash
# On your local machine, build the frontend
cd GradConnect-frontend/alumni-network-glow-1
npm run build

# Upload dist folder to your server
scp -r dist/* user@your-server-ip:/var/www/gradconnect

# Configure nginx to serve static files
# Update nginx config to point to /var/www/gradconnect
```

---

## Part 3: Important Configuration Steps

### MongoDB Setup (Required for both local and production):
1. Create a MongoDB Atlas account at https://www.mongodb.com/cloud/atlas
2. Create a cluster
3. Create a database user with strong password
4. Whitelist your IP address
5. Get your connection string
6. Update MONGO_URI in `.env` files

### Environment Variables Checklist:

**Backend (.env)**:
- [ ] PORT=5000
- [ ] MONGO_URI=your_mongodb_connection_string
- [ ] JWT_SECRET=strong_random_secret_key
- [ ] NODE_ENV=production (for production)
- [ ] FRONTEND_URL=your_frontend_url (for CORS)

**Frontend (.env.production)**:
- [ ] VITE_API_URL=your_backend_api_url/api

### CORS Configuration:
The backend already supports multiple origins. For production, update `server.js` CORS whitelist with your frontend URL.

---

## Part 4: Testing the Connection

1. Open frontend on http://localhost:5173 (or your deployed URL)
2. Go to Auth page
3. Try to register a new account:
   - Email: test@example.com
   - Password: Test123!
   - First Name: Test
   - Last Name: User
   - User Type: Alumni
4. Check if it redirects to profile setup
5. Check MongoDB to confirm user was created

---

## Part 5: Troubleshooting

### Frontend can't connect to backend:
- Check if backend is running on port 5000
- Check `VITE_API_URL` in frontend .env files
- Check browser console for CORS errors
- Ensure CORS is properly configured in server.js

### MongoDB connection fails:
- Verify MONGO_URI in .env
- Check if your IP is whitelisted in MongoDB Atlas
- Verify database user credentials
- Ensure cluster is active

### Authentication not working:
- Check if JWT_SECRET is set in backend .env
- Verify password hashing is working (check server logs)
- Check if token is being stored in localStorage

### Port conflicts:
```bash
# Find what's running on port 5000
lsof -i :5000

# Kill the process
kill -9 <PID>
```

---

## Part 6: Scaling & Optimization

1. **Add Redis** for session management
2. **Implement rate limiting** to prevent abuse
3. **Add request logging** with Morgan
4. **Set up SSL certificates** (free via Let's Encrypt)
5. **Enable database indexing** for frequently queried fields
6. **Implement JWT refresh tokens** for better security
7. **Add file upload handling** for profile pictures
8. **Implement email verification** during registration

---

## Quick Command Reference

**Backend Development**:
```bash
cd GradConnect-backend
npm install
npm run dev  # Requires nodemon
```

**Frontend Development**:
```bash
cd GradConnect-frontend/alumni-network-glow-1
npm install
npm run dev
```

**Build Frontend**:
```bash
npm run build  # Creates optimized dist folder
```

**Production Backend**:
```bash
npm start  # Uses node server.js
```

---

## Support & Next Steps

1. Implement more API endpoints (user profiles, chat, events, etc.)
2. Add OAuth (Google, LinkedIn) authentication
3. Set up CI/CD pipeline with GitHub Actions
4. Add comprehensive error handling and logging
5. Implement user profile customization
6. Add real-time features with WebSockets

For issues, check server logs and browser console for detailed error messages.
