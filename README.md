# Charan Logistics — Supabase Migration Guide

## GitHub Secrets needed (delete old ones, add these 2)

| Secret name        | Value                                      |
|--------------------|--------------------------------------------|
| `SUPABASE_URL`     | Your project URL from Supabase dashboard   |
| `SUPABASE_ANON_KEY`| Your anon/public key from Supabase         |

---

## Step 1 — Create Supabase project

1. Go to https://supabase.com → Sign up (free)
2. Click **New Project**
3. Name: `charan-logistics`
4. Set a strong database password (save it somewhere safe)
5. Region: **Southeast Asia (Singapore)** — closest to you
6. Click **Create new project** → wait ~2 minutes

---

## Step 2 — Create tables and enable security

1. In your project → click **SQL Editor** (left sidebar)
2. Click **New query**
3. Copy the entire contents of `supabase_setup.sql` and paste it
4. Click **Run**
5. You should see: `Success. No rows returned`

---

## Step 3 — Import your data from Google Sheet

### Option A — CSV import (easiest)

1. Open your Google Sheet → **File → Download → CSV** (do this for both Clients and LineItems tabs)
2. In Supabase → **Table Editor** → click `clients` table → **Insert → Import data from CSV**
3. Upload your Clients CSV → map columns:

| CSV column       | Supabase column  |
|------------------|------------------|
| invoice_number   | invoice_number   |
| name             | name             |
| address          | address          |
| billing_date     | billing_date     |
| due_date         | due_date         |
| status           | status           |
| total            | total            |

4. Repeat for `line_items` table with LineItems CSV

### Option B — Manual entry via Table Editor
For small datasets, just click **Insert row** in the Table Editor and type values directly.

---

## Step 4 — Create user accounts for your 4 people

1. In Supabase → **Authentication** → **Users** → **Add user**
2. Create one account per person:
   - Your email + password
   - Wife's email + password
   - Friend's email + password
   - Friend's wife's email + password
3. Each person logs in with their own email/password
4. **Disable "Confirm email"** in Auth → Settings → Email Auth → uncheck "Enable email confirmations" (so they can log in straight away without email verification)

---

## Step 5 — Get your credentials

1. In Supabase → **Project Settings** (gear icon) → **API**
2. Copy:
   - **Project URL** → this is your `SUPABASE_URL`
   - **anon / public** key → this is your `SUPABASE_ANON_KEY`

> The anon key is safe to use in the browser. It has no power without a valid logged-in user (enforced by Row Level Security).

---

## Step 6 — Add GitHub Secrets

1. GitHub repo → **Settings → Secrets and variables → Actions**
2. Delete old secrets: `ACCESS_HASH`, `APPS_SCRIPT_URL` (if they exist)
3. Add new secrets:
   - `SUPABASE_URL` → paste Project URL
   - `SUPABASE_ANON_KEY` → paste anon key

---

## Step 7 — Deploy

Replace your repo files with the new versions:
- `login.html` → new Supabase login
- `index.html` → Supabase data fetching
- `.github/workflows/deploy.yml` → updated workflow
