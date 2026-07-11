# GetProfile.me - API Specification

**Version:** 0.1 | **Date:** July 11, 2026

## Authentication
- Clerk JWT for protected routes
- LinkedIn OAuth2 for profile import

## Endpoints

### Onboarding & Profile
**POST /api/onboarding/generate**
- Body: `{ firstName, lastName, linkedinToken? , manualData? }`
- Returns: Generated profile (short resume HTML + JSON)

**GET /api/profile/[uid]**
- Public endpoint for rendered profile page

### Domain
**GET /api/domain/suggest**
- Query: `?firstName=...&lastName=...`
- Returns: List of available domains with prices

**POST /api/domain/register**
- Body: `{ domain, profileId }`
- Handles payment + registration + DNS setup

### Connections
**GET /api/connections**
- Returns: `{ incoming: [], outgoing: [] }`

**POST /api/connections/share**
- Share profile with another UID

### AI
**POST /api/ai/generate-resume**
- Body: LinkedIn data or manual input
- Uses Grok API prompt for short, effective one-page version

## Rate Limiting & Security
- 100 req/min per user
- Input validation (Zod)
- RLS on Supabase

## Error Handling
Standard JSON errors with codes.