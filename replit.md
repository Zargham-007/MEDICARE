# MediCare Clinic Management

Full-stack clinic management web app built on the Replit pnpm monorepo.

## Stack
- **Frontend:** React 19 + Vite 7 + TypeScript + Tailwind + shadcn/ui + wouter + TanStack Query + Recharts + Framer Motion + react-hook-form + zod
- **3D / Animation:** @react-three/fiber, @react-three/drei, three (with WebGL error boundaries for graceful degradation)
- **Backend:** Express 5 (`artifacts/api-server`) generated from OpenAPI spec
- **Database:** PostgreSQL via Drizzle ORM (`lib/db`)
- **Codegen:** `lib/api-spec/openapi.yaml` → typed client hooks (`@workspace/api-client-react`) and zod schemas (`@workspace/api-zod`)

## Artifacts
- `artifacts/clinic` — Patient-facing clinic web app (path: `/`)
- `artifacts/api-server` — REST API (mounted under `/api`)
- `artifacts/mockup-sandbox` — Component preview sandbox

## Domain model
- **Patients** — demographics, contact, medical history, allergies, blood type
- **Doctors** — profile, specialty, license, bio, availability
- **Appointments** — patient ↔ doctor, scheduled time, duration, status (scheduled/checked-in/completed/cancelled), reason, notes
- **Prescriptions** — patient ↔ doctor, medication, dosage, frequency, duration, refills, status

## Pages
- `/` — Landing page with 3D animated Three.js hero
- `/dashboard` — Stats, charts (appointments by day, department load), recent activity, upcoming appointments
- `/patients` and `/patients/:id` — list (search) + profile detail with full CRUD
- `/doctors` and `/doctors/:id` — directory (specialty filter) + profile with full CRUD
- `/appointments` — schedule + status filter + transitions (check-in, complete, cancel) + CRUD
- `/prescriptions` — list + issuance dialog + CRUD

## Backend endpoints
- `GET /api/healthz`
- CRUD on `/api/patients`, `/api/doctors`, `/api/appointments`, `/api/prescriptions`
- Dashboard: `/api/dashboard/{stats,upcoming-appointments,recent-activity,department-load,appointments-by-day}`

## Seeding
Run: `pnpm --filter @workspace/scripts run seed-clinic`
Seeds 6 doctors, 8 patients, 14 appointments, 5 prescriptions.

## Development
- `pnpm --filter @workspace/api-server run dev` — API server
- `pnpm --filter @workspace/clinic run dev` — Web app
- `pnpm --filter @workspace/api-spec run codegen` — Regenerate client/zod after editing OpenAPI spec
- `pnpm --filter @workspace/db run db:push` — Apply schema changes

## Notes
- Original request was for .NET; environment is Node.js/TS, so React + Vite + Express was used instead.
- 3D scenes are wrapped in `WebGLErrorBoundary` so environments without GPU/WebGL fall back to a gradient placeholder.
- Wouter routing uses `base={import.meta.env.BASE_URL.replace(/\/$/, "")}` for path-prefix support.
