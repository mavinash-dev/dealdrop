# DealDrop — Claude Code Context File

## ⚠️ FIRST ACTION EVERY SESSION
**Read `DEVELOPMENT_PLAN.md` before writing any code.**  
It contains the active story, session log, and all acceptance criteria.  
After reading, continue from the story marked 🔄 In Progress.

---

## Project Overview
DealDrop is a community-driven personal coupon vault and deal discovery platform for Indian consumers. Users save their own coupons, share deals with the community, and discover fresh affiliate deals — all in one place.

## Owner
- **Founder:** Avinash (mavinash-dev)
- **Email:** mavinash.dev@gmail.com
- **GitHub:** https://github.com/mavinash-dev

## Tech Stack
- **Frontend:** React 18 + Vite + Tailwind CSS + React Router + Zustand + React Query
- **Backend:** Supabase (Postgres + Auth + Realtime)
- **Hosting:** Vercel
- **Affiliate:** EarnKaro API, VCommission, CueLinks
- **AI:** Google Gemini API (deal discovery + categorization), Claude API (validation + recommendations), Gemini Gems (user-facing AI assistant)
- **Agents:** Vercel Cron Jobs trigger AI deal discovery agents hourly from Reddit, affiliate feeds, brand pages

## Project Structure
```
dealdrop/
├── src/
│   ├── components/     ← Reusable UI components
│   ├── pages/          ← Route-level page components
│   ├── hooks/          ← Custom React hooks
│   ├── store/          ← Zustand state stores
│   └── lib/            ← Supabase client, utils, helpers
├── supabase/
│   └── migrations/     ← SQL migration files
├── docs/               ← All project documentation
└── public/             ← Static assets
```

## Common Commands
```bash
npm run dev       # Start dev server on port 5173
npm run build     # Production build
npm run preview   # Preview production build
npm run lint      # ESLint check
```

## Database Tables
- `users` — authenticated users
- `coupons` — personal vault coupons (private or public)
- `community_deals` — publicly shared deals
- `votes` — upvotes/downvotes on community deals
- `comments` — comments on deals

## Key Conventions
- Use Tailwind CSS only — no custom CSS files
- All Supabase calls go through `src/lib/supabase.js`
- State management via Zustand in `src/store/`
- Server state (DB data) via React Query
- Component names: PascalCase
- File names: kebab-case
- Never commit `.env` files — use `.env.example` as template

## Environment Variables Needed
```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
VITE_EARNKARO_API_KEY=
```

## Current Phase
MVP v1.0 — Building core personal vault + community feed

## Docs Reference
- PRD: dealdrop_prd.md
- Tech Stack: tech_stack.md
- Costs: cost_breakdown.md
- GTM: gtm_strategy.md
- Roadmap: roadmap.md
- Open Source: opensource_strategy.md
