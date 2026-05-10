# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.

## Hydes Garage band site

The `web` artifact is a band site for Hydes Garage with a public side (home, shows, music, about) and an admin module.

### Admin login
- URL: `/admin/login`
- Default credentials are stored in shared env vars `ADMIN_USERNAME` and `ADMIN_PASSWORD` and can be changed in the Secrets pane.
- Sessions use `express-session` with the existing `SESSION_SECRET` secret. Cookies are HTTP-only.

### Domain tables (`lib/db/src/schema`)
- `shows` — live schedule (title, venue, city, date, ticketUrl, notes)
- `albums` — discography albums (title, releaseDate, description, coverUrl, listenUrl)
- `singles` — discography singles (same shape as albums)
- `about` — single-row band metadata (bandName, tagline, story, hometown, foundedYear, photo + social URLs)

### API surface (`/api`)
- `GET /api/healthz`
- Auth: `POST /api/auth/login`, `POST /api/auth/logout`, `GET /api/auth/me`
- Shows: `GET /api/shows`, `GET /api/shows/upcoming`, `GET /api/shows/:id`, plus admin-only `POST/PATCH/DELETE`
- Albums and Singles: same shape as Shows
- About: `GET /api/about`, admin-only `PUT /api/about`
- `GET /api/summary` — aggregate counts and next show
