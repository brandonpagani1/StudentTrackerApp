# Student Assignment Tracker
CPAN 366 - Project - By BATmen - Alexander Watson, Ibrahim Hagi, Tsering Lama, and Brandon Pagani Lozano

> **This repo has moved.** Development, CI/CD, and deploys now happen at
> [watts0nlol/s](https://github.com/watts0nlol/s). Please clone/push there going forward.

## Setup

### Prerequisites

- Node.js `^20.19.0` or `>=22.12.0` (required by Vite 8). Check your version with `node -v`.
  - On Windows, you can upgrade with `winget upgrade --id OpenJS.NodeJS.20`.
  - If you upgrade Node after already running `npm install`, delete `node_modules` and run `npm install` again — otherwise platform-specific native bindings (e.g. for Vite/Rolldown) may be missing and `npm run dev`/`npm run build` will fail with a `MODULE_NOT_FOUND` error.

### Install

```bash
npm install
```

### Environment variables

Copy `.env.example` to `.env` and fill in the values (JWT secret, email credentials, etc.):

```bash
cp .env.example .env
```

### Running the app

The frontend and backend run as separate processes:

```bash
# Frontend (Vite dev server)
npm run dev

# Backend (Express + Socket.IO API)
node server/index.js
```

## CI/CD

Every push/PR to `main` runs lint + build via GitHub Actions (`.github/workflows/ci-cd.yml`).
On push to `main`, it also triggers a backend deploy on Render.

The frontend is deployed separately by **Vercel's own GitHub integration** (connected directly
to whichever repo/branch the project was imported from on vercel.com) — it builds and deploys
automatically on every push, independent of this repo's GitHub Actions workflow.

- **Frontend → Vercel** (static `dist/` build, deployed via Vercel's native git integration)
- **Backend → Render** (Express/Socket.IO server, via `render.yaml` + GitHub Actions deploy hook)

### One-time setup (per maintainer)

1. **Render (backend)**
   - Create a new Web Service on [Render](https://render.com), connecting this repo. Render will pick up `render.yaml` automatically.
   - In the Render dashboard, set the `JWT_SECRET`, `EMAIL_USER`, `EMAIL_PASS` env vars on the service.
   - Under the service's **Settings → Deploy Hook**, copy the deploy hook URL.
   - In this GitHub repo, add it as secret `RENDER_DEPLOY_HOOK_URL` (Settings → Secrets and variables → Actions).
   - Note the service's public URL (e.g. `https://student-tracker-api.onrender.com`) — needed below.

2. **Vercel (frontend)**
   - On [vercel.com](https://vercel.com), import the repo you want Vercel to deploy from (note: if you
     don't have admin rights on the upstream repo, Vercel may create its own copy under your account —
     check which repo it's actually watching under Project Settings → Git).
   - Add env var `VITE_API_URL` = your Render backend URL from step 1, in that Vercel project's
     Settings → Environment Variables.
   - No GitHub secrets are needed for Vercel — it deploys directly, not through Actions.

Once both are set up, every push auto-deploys: Render via the Actions deploy hook on this repo,
Vercel via its own git integration on whichever repo it's connected to.

> Note: the backend currently stores data in memory (see `server/models/`), so every backend redeploy/restart resets all data. Fine for a class project demo; would need a real database for persistent data.
