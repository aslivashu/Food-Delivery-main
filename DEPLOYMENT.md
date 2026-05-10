# Deployment Guide — Vercel (frontend) + Render (backend) + MongoDB Atlas (free)

Overview
- Frontend: deploy the `frontend` folder as a static site (Vercel or Netlify).
- Backend: deploy the `backend` folder as a Node web service (Render or Railway).
- Database: use MongoDB Atlas free tier; set `MONGO_URL` in backend env.

Prerequisites
- Push this repository to GitHub (or another Git provider).
- Create accounts: Vercel (frontend), Render (backend), MongoDB Atlas (DB).

MongoDB Atlas (quick)
1. Sign in to MongoDB Atlas and create a free cluster.
2. Create a database user and password.
3. Allow access (use your IP or 0.0.0.0/0 for quick testing).
4. Copy the connection string and replace `<password>` with the user password.
5. Set the connection string as `MONGO_URL` in your backend service's env.

Backend — Render (recommended free option)
1. In GitHub, ensure the repo is up to date and accessible.
2. On Render: New → Web Service → Connect GitHub repo.
3. Set the root/directory to `backend`.
4. Build & start: Render will install; set the start command to `npm start` (already in `backend/package.json`).
5. Add environment variables (from `backend/.env.example`):
   - `MONGO_URL` (Atlas connection string)
   - `JWT_SECRET` (secure random string)
   - `SALT` (e.g. `10`)
   - `STRIPE_SECRET_KEY` (Stripe test key for testing)
   - `FRONTEND_URL` (deployed frontend URL, used for Stripe success/cancel)
6. Deploy and note the backend URL (e.g., `https://your-backend.onrender.com`).

Frontend — Vercel
1. On Vercel: New Project → Import GitHub repo.
2. Set the root directory to `frontend`.
3. Build command: `npm run build` (Vercel will run `npm install`).
4. Output directory: `dist`.
5. Add environment variable `VITE_API_URL` = your backend URL (from Render).
6. Deploy. Copy the frontend URL once deployment finishes.

Local testing
1. Backend:
```bash
cd backend
cp .env.example .env
# edit .env to fill values
npm install
npm run server   # development (nodemon)
```
2. Frontend:
```bash
cd frontend
cp .env.example .env
# edit .env if needed (VITE_API_URL)
npm install
npm run dev
```

Notes & gotchas
- The backend serves images from `uploads/` (local filesystem). Most cloud hosts have ephemeral storage; use Cloudinary, AWS S3, or similar for persistent uploads in production.
- Stripe: use test keys (`STRIPE_SECRET_KEY`) for testing. Set `FRONTEND_URL` to the deployed frontend so checkout redirects work.
- Do not commit real secrets; use environment variables in the host dashboard.

Quick verification
- Backend root: `GET https://<backend-url>/` should return `API Working`.
- Frontend: open the deployed site and confirm food list loads and login/cart flows work.

If you want me to continue I can:
- Option A: Create a minimal GitHub Actions workflow to build and deploy the frontend automatically.
- Option B: Prepare Render / Vercel-specific config files or a `render.yaml`.
- Option C: Walk you step-by-step through the first deploy (I’ll give exact clicks and values).
