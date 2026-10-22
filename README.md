# Daily Schedule Tracker

React + TypeScript + Tailwind + Supabase. Three tabs: Today, History, Weekly progress.

## 1. Put in your schedule
Edit `src/data/schedule.ts`. It currently holds **sample data** — replace the rows with your real schedule
(`[start, end, title, details?]` in 24-hour time, per weekday). To continue past 31 Dec 2026, add another entry to `PERIODS`.

## 2. Set up Supabase (free tier is fine)
1. Create a project at supabase.com.
2. SQL Editor → paste and run `supabase/schema.sql` (creates the table, Row Level Security policies, realtime).
3. Project Settings → API → copy the **Project URL** and **anon public** key. Never use the `service_role` key.
4. Authentication → URL Configuration → set **Site URL** to your deployed URL (and add `http://localhost:5173` to Redirect URLs). Password-reset emails use this.
5. (Optional) Authentication → Providers → Email → turn off "Confirm email" if you don't want to confirm via email.

## 3. Run locally
```
cp .env.example .env     # fill in the two values
npm install
npm run dev
```

## 4. Deploy
**Vercel:** import the repo → Framework "Vite" → add env vars `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` → Deploy.
**Netlify:** build command `npm run build`, publish directory `dist`, same two env vars.
Then set your live URL as the Supabase Site URL (step 2.4).

## How it works
- Dates are `YYYY-MM-DD` in Asia/Dhaka; weeks run Saturday–Friday.
- One row per user per day in `day_logs`. The first time you check a task, that day's task list is saved as a snapshot, so later schedule edits never change past records.
- Optimistic updates with rollback + an error banner if a save fails. Realtime + refetch on focus keeps devices in sync. localStorage is only a cache.
- Days with no saved activity show "No record" and are left out of weekly stats and the chart; future days are never counted.
