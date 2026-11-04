# The AdSphere Awards Website

**Where Ideas Become Icons.**

A complete full-stack event website for **The AdSphere Awards** — a prestigious advertising, branding and creative awards ceremony.

- **Event Dates:** 20–21 February 2027  
- **Location:** Bengaluru, Karnataka, India  
- **Stack:** Next.js 15 (App Router) + TypeScript + Tailwind CSS + Prisma + Neon PostgreSQL + NextAuth + Resend + QR Codes  
- **Deployment:** Vercel  

---

## Features Implemented

### Public Website
- Premium dark + gold award-night design
- Homepage with countdown timer, hero, categories preview, two-day overview
- About, Categories, Submit Entry, Register to Attend, Nominees, Speakers, Sponsors, Agenda, Venue, Contact
- Legal pages (Privacy, Terms, Submission Terms)
- Fully responsive

### Award Entry System
- Full multi-field campaign submission form with validation (Zod + React Hook Form)
- Unique Submission ID generation
- Status workflow (Submitted → Under Review → Shortlisted etc.)
- Confirmation email via Resend
- Duplicate protection

### Attendee Registration + QR Tickets
- Separate registration form (independent of award submissions)
- Unique Registration ID + Ticket Number
- Secure QR code generation (token-based, no PII in QR)
- Confirmation email
- Duplicate email prevention

### Administration
- Role-based authentication (NextAuth + Credentials)
- Roles: SUPER_ADMIN, EVENT_MANAGER, AWARD_COORDINATOR, JURY_MEMBER, etc.
- Dashboard with live stats from database
- Modules structure for submissions, registrations, categories, sponsors, speakers, agenda

### Database (Prisma + Neon)
- Comprehensive normalised schema covering:
  - Users & Roles
  - Award Categories & Submissions + status history + files
  - Jury members, assignments, evaluations & scoring criteria
  - Nominees / Finalists / Winners
  - Attendees, Tickets, Check-ins
  - Sponsors, Speakers, Artists, Event Sessions
  - SPOCs, Email Templates & Logs, Audit Logs, Media Assets

### Email (Resend)
- Template system with variable replacement
- Confirmation emails for submissions and registrations
- Email logging

---

## Quick Start (Local Development)

### 1. Prerequisites
- Node.js 18+
- A Neon PostgreSQL database (free tier is fine)
- (Optional) Resend account for emails

### 2. Install
```bash
cd The_AdSphere_Awards_Website
npm install
```

### 3. Environment
```bash
cp .env.example .env.local
```

Edit `.env.local`:

```
DATABASE_URL="postgresql://USER:PASSWORD@HOST/neondb?sslmode=require"
AUTH_SECRET="generate-with-openssl-rand-base64-32"
NEXT_PUBLIC_APP_URL="http://localhost:3000"
RESEND_API_KEY="re_xxxx"          # optional for local
RESEND_FROM_EMAIL="noreply@yourdomain.com"
RESEND_FROM_NAME="The AdSphere Awards"
```

### 4. Database
```bash
npx prisma generate
npx prisma db push
npm run db:seed
```

Default admin credentials after seed:
- **Email:** `admin@adsphereawards.com`
- **Password:** `Admin@AdSphere2027`

**Change this password immediately in production.**

### 5. Run
```bash
npm run dev
```

Open http://localhost:3000

Admin: http://localhost:3000/admin/login

---

## Deployment to Vercel + Neon

1. Push this project to a **GitHub** repository.
2. Go to [vercel.com](https://vercel.com) → New Project → Import the repo.
3. Create a Neon database at [console.neon.tech](https://console.neon.tech).
4. In Vercel project settings → Environment Variables, add:
   - `DATABASE_URL` (Neon connection string – use the **pooled** one for serverless)
   - `AUTH_SECRET`
   - `NEXT_PUBLIC_APP_URL` (your Vercel domain, e.g. https://adsphere.vercel.app)
   - `RESEND_API_KEY`, `RESEND_FROM_EMAIL`, `RESEND_FROM_NAME`
5. Deploy. Vercel will run `prisma generate` via the `postinstall` / `build` script.
6. After first deploy, run migrations/seed if needed (you can use `npx prisma db push` from a local machine pointed at the production `DATABASE_URL`, or use Neon SQL editor + seed carefully).

---

## Project Structure

```
The_AdSphere_Awards_Website/
├── prisma/
│   ├── schema.prisma      # Full database schema
│   └── seed.ts            # Admin user + categories + templates
├── src/
│   ├── app/
│   │   ├── page.tsx               # Homepage
│   │   ├── about, categories, submit, register, ...
│   │   ├── admin/                 # Dashboard + login
│   │   ├── api/
│   │   │   ├── categories/
│   │   │   ├── submissions/
│   │   │   ├── registrations/
│   │   │   └── auth/[...nextauth]/
│   │   └── legal/
│   ├── components/        # Navbar, Footer, Countdown
│   └── lib/               # prisma, auth, email, utils
├── .env.example
├── package.json
├── vercel.json
└── README.md
```

---

## Important Notes for Students / First-time Deployers

- **Never commit `.env` or `.env.local`** — they contain secrets.
- The public site and admin share the same Neon database. Changes in admin (once full CRUD pages are extended) appear on the public site.
- Speakers, sponsors, nominees and venue details start empty / unpublished. Only published records should be shown publicly.
- QR codes contain a verification token only — no personal data.
- File uploads for campaign assets can be added later with a service such as Uploadthing, Cloudinary or Vercel Blob. The schema already has `SubmissionFile` and `MediaAsset` models ready.
- Jury evaluation UI and full admin CRUD for every module can be extended on top of the existing Prisma models and auth system.
- For production email, verify your domain in Resend.

---

## Default Admin Account

```
Email:    admin@adsphereawards.com
Password: Admin@AdSphere2027
```

**Change immediately after first login.**

---

## Licence & Credits

Built specifically for **The AdSphere Awards – Where Ideas Become Icons.**

This project is provided as a complete starting point and functional core system for a college / student deployment exercise.
