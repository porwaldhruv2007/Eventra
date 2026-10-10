# Eventra — Event Registration & Venue Scheduling

Eventra is a web application for publishing events, registering participants, managing venue bookings, and tracking attendance. Organizers and administrators can manage operations from a dashboard with event, registration, attendance, and venue-utilization reports.

[Printable project guide (PDF)](src/Eventra-Project-Guide.pdf) · [Source code guide](src/README.md)

## Features

- **Event management:** Create and manage events, assign venues, and set event capacity and status.
- **Venue scheduling:** Manage venues and bookings. Availability checks account for confirmed bookings, scheduled events, venue opening hours, venue status, and scheduled or in-progress maintenance.
- **Registration and tickets:** Let participants register for events and receive a registration code that can be rendered as a QR code.
- **Attendance tracking:** Check participants in by scanning a ticket QR code or mark attendance manually.
- **Role-based dashboards:** Separate capabilities for administrators, organizers, and participants.
- **Reports and notifications:** Review registration, attendance, event-capacity, and confirmed venue-booking data; receive in-app notifications.

Availability is checked by the application when a booking or event is scheduled; it is not a real-time synchronization service.

## Tech stack

| Area | Technology |
| --- | --- |
| Framework | Next.js 16 (App Router) |
| Language | TypeScript 5 |
| UI | React 19, Tailwind CSS 4 |
| Database | PostgreSQL, with Drizzle ORM and Drizzle Kit |
| Database hosting | Supabase PostgreSQL can be used via `DATABASE_URL` |
| QR codes | `qrcode` |
| Linting | ESLint 9 |

## Requirements

- Node.js compatible with the installed Next.js version
- npm
- A PostgreSQL database and its connection string

## Getting started

1. Clone the repository and enter the project directory:

   ```bash
   git clone https://github.com/porwaldhruv2007/Eventra.git
   cd Eventra
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create `.env.local` from the example file (`Copy-Item .env.example .env.local` in PowerShell, or `cp .env.example .env.local` in macOS/Linux). Set `DATABASE_URL` to your PostgreSQL connection string. If using Supabase, get the connection string from your project's database settings. The example also contains Supabase browser-client settings; replace their placeholder values with your project's URL and publishable key if you use that client.

   Keep `.env.local` private and do not commit database credentials or other secrets.

4. Apply the Drizzle migrations to the configured database:

   ```bash
   npm run db:migrate
   ```

5. Start the development server:

   ```bash
   npm run dev
   ```

   Open [http://localhost:3000](http://localhost:3000). Create an account at `/register`; registration supports the participant and organizer roles.

## Available scripts

| Command | Description |
| --- | --- |
| `npm run dev` | Start the Next.js development server |
| `npm run build` | Create a production build |
| `npm run start` | Start the production server (run `npm run build` first) |
| `npm run lint` | Run ESLint |
| `npm run typecheck` | Run the TypeScript compiler without emitting files |
| `npm run db:generate` | Generate a Drizzle migration from schema changes |
| `npm run db:migrate` | Apply pending Drizzle migrations |

## Project structure

```text
src/
  app/          Next.js routes, dashboards, and API endpoints
  components/   Shared forms and UI components
  db/           Drizzle schema, database connection, and seed data
  lib/           Server actions, authentication, queries, and utilities
drizzle/         Generated SQL migrations
public/          Static assets and SQL scripts
```

The app's PostgreSQL tables are defined in `src/db/schema.ts` and created by the migrations in `drizzle/`. The standalone `public/eventra.html` and its associated Supabase SQL scripts are separate from the Next.js app's Drizzle-backed user and event data.

## API endpoints

- `POST /api/availability` — Check whether a venue is available for a requested time. Requires a signed-in user.
- `GET /api/qr?data=...` — Return an SVG QR code for the supplied data.
- `GET /api/health` — Check whether the app can reach its database.
