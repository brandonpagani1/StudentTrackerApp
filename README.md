# Student Assignment Tracker
CPAN 366 - Project - By BATmen - Alexander Watson, Ibrahim Hagi, Tsering Lama, and Brandon Pagani Lozano

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
