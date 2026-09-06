# Project Plan

## Overview
This project will scaffold a full-stack solution for the SIH 2026 workspace using:

- **Next.js 14** with the App Router, TypeScript, and Tailwind CSS for the frontend
- **FastAPI** with **SQLite** for the backend
- A **BFF layer** in Next.js under `/api/*` for middleware-style API routing

## Architecture

### Frontend
- Next.js 14 App Router
- TypeScript-first implementation
- Tailwind CSS for styling
- Mobile-first UI with saffron-white-green inspired theme
- Glassmorphism cards and accessible components

### Backend
- FastAPI REST API
- SQLite database for easy local persistence
- Endpoints for auth, profile lookup, documents, and fulfillment

### Integration
- Frontend and backend communicate over REST
- Next.js API routes proxy or orchestrate calls where needed
- Clear separation between UI, assistant, and rules engine

## AI Assistant

- Primary route through `/api/chat` in Next.js
- Integrates with **Gemini Flash** for multilingual profile extraction when API key is available
- Includes deterministic regex fallback parser when no AI key is present
- Enables the demo to work in both real and offline/mock modes

## Rules Engine

- Fully deterministic; no LLM involvement in eligibility decisions
- JSON AST to model rule logic with:
  - `AND`, `OR`, `NOT`
  - 9 leaf operators for scheme eligibility checks
- Designed for extensibility and auditability
- Unit tested with `pytest`

## Auth & Fulfillment

### MeriPehchaan Mock OAuth
- Generates a signed session token
- Returns a masked Aadhaar-style profile

### DigiLocker Mock
- Returns verified certificates such as:
  - Income certificate
  - Caste certificate
  - Land Khatauni

### CSC Dispatch
- Generates a support ticket
- Simulates a WhatsApp geo-push notification to the nearest agent

## UI Experience

- Mobile-first layout with strong accessibility
- Floating multilingual chat widget
- Voice input support via Web Speech API
- Assist Mode toggle between:
  - DigiLocker auto-fill
  - CSC agent doorstep support

## Open Questions

1. **API Key**
   - Do you have a `GEMINI_API_KEY` or `OPENAI_API_KEY`?
   - Or should I use the mock regex parser for the AI assistant demo?

2. **Map in CSC Modal**
   - Prefer a real interactive map using Leaflet.js with mock pins?
   - Or a static placeholder map for faster delivery?

3. **i18n**
   - Should I implement full `react-intl` translations for EN/HI/TA?
   - Or keep a simpler static UI language toggle?

## Next Steps

Once you confirm the open questions, I can scaffold the frontend and backend, wire the assistant, add deterministic eligibility logic, and seed the demo data.
