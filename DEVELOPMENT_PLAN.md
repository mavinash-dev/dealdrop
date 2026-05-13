# DealDrop — Dev Plan Index

## 🤖 Claude — Session Protocol (read every time)
1. Read this file top to bottom (~50 lines, fast).
2. Find the 🔄 story below — note its sprint file in the Sprint Files table.
3. Read ONLY that sprint file. Do not load others.
4. Build the story. When done:
   - Mark it ✅ in the table below.
   - Set next story to 🔄.
   - Update `## Current Status`.
   - Add one row to `## Session Log`.
5. To resume: Avinash says *"Read DEVELOPMENT_PLAN.md and continue."*

---

## Current Status
| Field | Value |
|---|---|
| **Active Story** | S01 — Project Scaffold |
| **Sprint File** | `sprints/sprint-1-foundation.md` |
| **Last Session** | *(none — first session)* |
| **Blocked?** | No |

---

## Session Log
| Date | Stories Done | Summary | Next |
|---|---|---|---|
| *(none yet)* | — | — | S01 |

---

## Sprint Files
| Sprint | Theme | File | Stories |
|---|---|---|---|
| 1 | Foundation | `sprints/sprint-1-foundation.md` | S01–S04 |
| 2 | Personal Vault | `sprints/sprint-2-vault.md` | S05–S08 |
| 3 | Community Feed | `sprints/sprint-3-community.md` | S09–S13 |
| 4 | AI Agents | `sprints/sprint-4-ai-agents.md` | S14–S17 |
| 5 | Gems & Monetization | `sprints/sprint-5-gems.md` | S18–S20 |
| 6 | Polish & Launch | `sprints/sprint-6-launch.md` | S21–S24 |

---

## Story Status
| ID | Story | Sprint | Status |
|---|---|---|---|
| S01 | Project Scaffold | 1 | 🔄 In Progress |
| S02 | Supabase Schema & Migrations | 1 | 🔲 |
| S03 | Google Auth | 1 | 🔲 |
| S04 | App Layout & Navigation | 1 | 🔲 |
| S05 | Add Coupon Form | 2 | 🔲 |
| S06 | Coupon Card Component | 2 | 🔲 |
| S07 | Personal Vault Page | 2 | 🔲 |
| S08 | Edit & Delete Coupon | 2 | 🔲 |
| S09 | Community Feed Page | 3 | 🔲 |
| S10 | Share to Community | 3 | 🔲 |
| S11 | Upvote / Downvote | 3 | 🔲 |
| S12 | Mark Working / Expired | 3 | 🔲 |
| S13 | Search & Filter | 3 | 🔲 |
| S14 | EarnKaro Affiliate Feed | 4 | 🔲 |
| S15 | Gemini AI Deal Extraction | 4 | 🔲 |
| S16 | Reddit Scraper Agent | 4 | 🔲 |
| S17 | Vercel Cron — Agent Runner | 4 | 🔲 |
| S18 | Gemini Gems Chat Interface | 5 | 🔲 |
| S19 | Affiliate Link Auto-Embed | 5 | 🔲 |
| S20 | AI Duplicate Detection | 5 | 🔲 |
| S21 | Mobile Polish & PWA | 6 | 🔲 |
| S22 | SEO — Meta & OG Tags | 6 | 🔲 |
| S23 | Seed 100+ Real Deals | 6 | 🔲 |
| S24 | Vercel Deploy & Domain | 6 | 🔲 |

---

## Architecture (locked — don't change without Avinash)
| Decision | Choice |
|---|---|
| Server state | React Query |
| UI/auth state | Zustand |
| Styling | Tailwind only — no custom CSS |
| Supabase calls | Only via `src/lib/supabase.js` |
| Agent code | `src/agents/` |
| API/cron routes | `api/` (Vercel serverless) |
| File naming | kebab-case (`coupon-card.jsx`) |
| Component naming | PascalCase (`CouponCard`) |

## Out of Scope for MVP
Email/SMS auth · Comments UI · Telegram ingestion · Payments · Next.js · Any analytics beyond Vercel
