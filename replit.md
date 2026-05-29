# RAVEX BDE Management Tool

Internal web application for RAVEX — a website creation company. Enables Admin to monitor BDE activities and BDEs to manage leads, generate WhatsApp messages, and track daily targets.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm --filter @workspace/ravex run dev` — run the frontend (port assigned dynamically)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string
- Required env: `SESSION_SECRET` — session signing secret

## Default Credentials

| Role  | Employee ID | Password  |
|-------|------------|-----------|
| Admin | ADMIN001   | admin123  |
| BDE   | BDE001     | bde123    |
| BDE   | BDE002     | bde123    |

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite + Tailwind CSS, wouter for routing
- API: Express 5
- Auth: Session-based (express-session + bcryptjs), Employee ID + Password
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — OpenAPI contract (source of truth)
- `lib/db/src/schema/` — Drizzle table definitions (employees, leads, followups, progress, activities)
- `artifacts/api-server/src/routes/` — Express route handlers (auth, employees, leads, followups, progress, activities, dashboard)
- `artifacts/ravex/src/` — React frontend (pages, components, AuthContext)

## Architecture decisions

- Session auth with express-session: Employee ID + password login, session stored server-side, no JWT complexity
- Roles enforced server-side: requireAuth and requireAdmin middleware in routes
- Activities auto-logged: lead creation, status changes, follow-up creation all write to activities table automatically
- Dark mode only: app is locked to dark theme, no toggle needed
- WhatsApp generator is purely client-side: no API call, messages generated locally in the browser

## Product

- **Login**: Employee ID + Password with RAVEX branding (logo, tagline)
- **BDE Dashboard**: Today's progress cards with targets (100 msgs / 30 leads / 5 interested / 1 sale)
- **Lead Management**: Full CRUD with status tracking (new → contacted → interested → follow_up → closed)
- **Follow-Up Tracker**: Pending follow-ups with mark-as-done
- **WhatsApp Generator**: Professional message templates in English, Malayalam, Hindi
- **Daily Targets**: Progress bars, inline counter updates
- **Knowledge Center**: RAVEX package info and contact details
- **Admin Dashboard**: Org-wide stats, BDE performance table, activity feed
- **Employee Management**: Admin CRUD for employee accounts

## User preferences

- Keep the project lightweight and simple (Replit free tier)
- Dark theme only
- No complex animations or heavy graphics
- Mobile friendly

## Gotchas

- Always restart the API server workflow after backend changes (it builds then starts)
- Always run codegen after OpenAPI spec changes: `pnpm --filter @workspace/api-spec run codegen`
- Session cookie requires credentials: true on CORS and withCredentials on fetch (configured in api-client-react custom-fetch)

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
