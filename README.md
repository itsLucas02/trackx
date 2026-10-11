# TrackX

[![Live demo](https://img.shields.io/badge/live-demo-4f8cff?style=flat-square)](https://trackx-three.vercel.app/)

**Report and discuss local community safety incidents on a shared map and board.**

TrackX is a community platform where residents report and discuss local safety
incidents — theft, vandalism, hazards, suspicious activity, and lost & found —
on a shared map and a discussion board. Reports describe events and behaviour,
**never any person's ethnicity, religion, or nationality.**

Bilingual (English + Bahasa Malaysia), mobile-first, and installable as a PWA.

## Features

- **Map-first interface** — MapLibre GL + `react-map-gl` with monochrome CARTO
  basemaps (light/dark), clustered markers (`supercluster`), and a mobile
  draggable glass bottom sheet.
- **Community board** — Newest / Trending / Near-me feeds, with live stats.
- **Create a report** — title, description, category, tap-the-map location, and
  up to 3 photos (compressed client-side, uploaded directly to Cloudflare R2 via
  presigned URLs). A posting-guidelines gate is shown before posting.
- **Engagement** — upvotes, comments, and flagging (3 flags auto-hide a report).
- **Notifications** — realtime bell powered by Supabase Realtime.
- **Auth & profiles** — Supabase email/password and Google OAuth.
- **Bilingual** — English + Bahasa Malaysia via `next-intl` (`/en`, `/ms`).
- **Theming** — light/dark via `next-themes`, following the TrackX design system.
- **PWA** — manifest, service worker, and offline page.
- **Privacy & Terms** pages.

## Tech stack

| Area | Tech |
|---|---|
| Framework | Next.js 16 (App Router, Turbopack dev) |
| UI | React 19, TypeScript, Tailwind CSS 4, shadcn/ui + Radix, lucide icons |
| Data & auth | Supabase (Postgres with a dedicated `safety` schema, RLS, Realtime) |
| Storage | Cloudflare R2 (S3-compatible) + presigned upload URLs |
| Maps | MapLibre GL / `react-map-gl` + `supercluster` |
| i18n | `next-intl` |
| Data fetching | TanStack Query |
| Validation | Zod |

## Getting started

### 1. Prerequisites

- Node.js 20+
- A Supabase project (self-hosted or hosted)
- A Cloudflare R2 bucket (with a public read URL)

### 2. Install

```bash
npm install
```

### 3. Configure environment

Create `.env.local` in the project root:

```bash
# Supabase
NEXT_PUBLIC_SUPABASE_URL=your-project-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key

# Cloudflare R2 (S3-compatible)
R2_ACCOUNT_ID=your-account-id
R2_ACCESS_KEY_ID=your-access-key-id
R2_SECRET_ACCESS_KEY=your-secret-access-key
R2_BUCKET=your-bucket-name
NEXT_PUBLIC_R2_PUBLIC_URL=https://your-public-bucket-url
```

> The app reads and writes through a dedicated **`safety`** schema (not `public`),
> so it can coexist with other projects on the same Supabase instance.

### 4. Set up the database

Apply the migrations in `supabase/migrations/` to your Supabase project:

```bash
supabase db push
```

Or run the SQL files (`001_safety_schema.sql`, `002_rls_perf.sql`) in the
Supabase SQL editor. They create the `safety` schema, tables, triggers, and RLS
policies, and enable Realtime on `safety.notifications`.

### 5. Run

```bash
npm run dev        # http://localhost:3000 (redirects to /en)
npm run build      # production build
npm start          # serve the production build
npm run lint       # eslint
```

## Project structure

```
src/
  app/[locale]/     Route groups: map, board, report, notifications,
                    profile, auth, privacy, terms
  app/api/upload/   Presigned R2 upload endpoint
  components/       UI (map, board, report, layout, auth, shadcn primitives)
  lib/              Supabase, R2, map config, images, actions, data access
  i18n/             next-intl routing + request config
  types/            Database types
messages/           en.json / ms.json translation strings
supabase/migrations Postgres schema, triggers and RLS
public/             PWA manifest, service worker, offline page, icons
design.md           TrackX design system (colors, spacing, layout rules)
```

## Content policy

Reports must describe incidents and behaviour, not people by ethnicity,
religion, or nationality. Targeting or harassing any group is grounds for
removal.

## License

Released under the MIT License. See [LICENSE](LICENSE).
