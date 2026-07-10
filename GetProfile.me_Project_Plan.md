# GetProfile.me Project Plan

**Current Date:** July 10, 2026

## Vision
A dead-simple service where users enter their first and last name and instantly get a full professional online business card website with custom domain, UID, and digital NFC card.

## Core Requirements

### Onboarding
- First Name + Last Name input
- LinkedIn Connect (Must-have)
- Manual fallback form (headline, bio, photo, etc.)
- AI converts LinkedIn data into short, effective one-page resume-style profile

### Domain
- Strong preference for firstname-lastname.com variations
- Auto-suggest + check availability + one-click registration
- Fallback: username.getprofile.me
- Permanent UID assigned to every profile

### App / PWA
- Home screen shows as clean business card
- When opened: Shows your profile + My Connections (bidirectional - who you shared with and who shared with you)

### Backend
- Pull LinkedIn profile
- AI transformation to concise getprofile.me version
- Domain management

## Next Steps
1. Setup Next.js project on port 5000
2. Build beautiful onboarding page
3. Implement LinkedIn OAuth
4. AI profile generator
5. Domain suggestion UI

Built with ❤️ for speed and simplicity.