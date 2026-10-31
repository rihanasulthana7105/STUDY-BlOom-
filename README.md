# StudyBloom – Smart Study Planner (React + Node.js + PostgreSQL)

## Features
Sign up / login (bcrypt-hashed passwords, server-side sessions stored in PostgreSQL), Google OAuth, forgot/reset password, dashboard, tasks, focus timer with saved history, weekly timetable, exam planner with countdowns, progress charts, settings (name, password, dark mode, daily goal). Every record has a `user_id` taken from the session, so users can only see and change their own data.

## Run locally
1. Install Node 18+ and PostgreSQL 14+.
2. `createdb studybloom`
3. `cp .env.example .env` then set `DATABASE_URL` and a long random `SESSION_SECRET`.
4. `npm install && npm run db:init` (needs `psql`; or run `schema.sql` in any SQL client).
5. Terminal 1: `npm run dev` (API on :3000). Terminal 2: `cd client && npm install && npm run dev` → open http://localhost:5173.

## Google Sign-In
Google Cloud Console → APIs & Services → Credentials → OAuth client ID (Web). Add redirect URI `http://localhost:5173/api/auth/google/callback` locally (and `https://YOUR-DOMAIN/api/auth/google/callback` in production). Put the ID and secret in `.env`.

## Password-reset email
Set `SMTP_URL` (e.g. `smtps://user:pass@smtp.example.com`) and `MAIL_FROM`. If empty, the reset link is printed in the server console.

## Deploy (Render, Railway, Fly.io, etc.)
1. Create a managed PostgreSQL database; run `schema.sql` on it.
2. Create a Node web service from this repo: build `npm install && npm run build`, start `npm start`.
3. Set env vars: `DATABASE_URL`, `SESSION_SECRET`, `NODE_ENV=production`, `APP_URL=https://YOUR-DOMAIN`, optional Google/SMTP vars, and `PGSSL=true` if your provider requires SSL.
4. The Express server serves the built React app and the API from one origin, so cookies work without CORS setup.

## Security notes
Secrets live only in environment variables; `.env` must stay out of git. Passwords use bcrypt (cost 12); sessions are httpOnly, sameSite=lax and secure in production; auth routes are rate-limited; all SQL is parameterized.
