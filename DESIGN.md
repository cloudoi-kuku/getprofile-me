# GetProfile.me - Detailed Design Document

**Last Updated:** July 11, 2026

## 1. System Architecture (High-Level)

```mermaid
graph TD
    A[User Browser / Mobile App] --> B[Next.js Frontend (App Router)]
    B --> C[Clerk Auth + LinkedIn OAuth]
    B --> D[Next.js API Routes / Server Actions]
    D --> E[Grok API / Azure OpenAI - Profile Generation]
    D --> F[Domain Registrar API]
    D --> G[Supabase / PostgreSQL DB]
    D --> H[Stripe - Payments & Domain Purchase]
    G --> I[Storage for Photos & Assets]
    B --> J[PWA + Web NFC]
```

### Key Layers
- **Frontend**: Next.js 15 + TypeScript + Tailwind + shadcn/ui
- **Auth**: Clerk (LinkedIn support)
- **Database**: Supabase (Postgres + Auth + Storage) or Azure Postgres
- **AI**: Grok API for profile condensation
- **Hosting**: Vercel (custom domains) or Azure Static Web Apps
- **PWA**: Next-PWA or manual manifest + service worker

## 2. Database Schema (PostgreSQL)

### Main Tables
```sql
-- Users
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  clerk_id TEXT UNIQUE,
  first_name TEXT,
  last_name TEXT,
  email TEXT,
  linkedin_id TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Profiles
CREATE TABLE profiles (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  uid TEXT UNIQUE NOT NULL,           -- e.g. uid_8f3k9p2m
  domain TEXT UNIQUE,                  -- e.g. bisi-kuku.com
  title TEXT,
  bio TEXT,
  photo_url TEXT,
  linkedin_data JSONB,
  short_resume_html TEXT,              -- AI generated one-page content
  status TEXT DEFAULT 'draft',
  created_at TIMESTAMP DEFAULT NOW()
);

-- Connections (Bidirectional)
CREATE TABLE connections (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  from_user_id UUID REFERENCES users(id),
  to_user_id UUID REFERENCES users(id),
  shared_at TIMESTAMP DEFAULT NOW(),
  notes TEXT,
  tags TEXT[]
);

-- Domains
CREATE TABLE domains (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  profile_id UUID REFERENCES profiles(id),
  domain_name TEXT UNIQUE,
  registrar TEXT,
  status TEXT DEFAULT 'pending',
  expires_at TIMESTAMP
);
```

## 3. API Endpoints (Next.js API Routes)

- `POST /api/onboarding/generate` — Name + LinkedIn → AI short profile
- `GET /api/domain/suggest?first=Bisi&last=Kuku` — Domain suggestions
- `POST /api/domain/register` — Purchase & bind domain
- `GET /api/profile/[uid]` — Render public profile
- `POST /api/linkedin/callback` — OAuth handler
- `GET /api/connections` — User's incoming/outgoing connections

## 4. Onboarding Flow (Detailed)

1. Landing → Name inputs
2. "Connect LinkedIn" (OAuth) or "Manual"
3. AI processes → Preview short resume page
4. Domain picker (variations of first-last)
5. Confirm → Create UID + Profile record + Deploy page

## 5. UI/UX Screens
- Home / Landing (hero + name form)
- Onboarding (LinkedIn + manual dialog)
- Profile Preview
- My Card (PWA home)
- My Connections (tabs: Incoming / Outgoing)
- Settings (domain, privacy)

## 6. PWA & NFC
- Manifest.json + service worker
- Web NFC API for tap
- Shareable vCard + link

## 7. Security & Compliance
- Clerk for auth
- Row Level Security in Supabase
- Consent for LinkedIn data
- HTTPS + rate limiting

---

This document will evolve as we build. Let me know what section to expand next (e.g., AI prompts, full API specs, UI wireframes, etc.).
