# 🛠️ DealDrop — Tech Stack Document
**Version:** 1.0
**Date:** May 13, 2026
**Author:** Avinash (mavinash-dev)

---

## Overview

DealDrop is built with a modern, lightweight, and cost-efficient stack. Every tool was chosen based on three criteria:
- **Free or very low cost** at MVP stage
- **Scalable** without rewriting when we grow
- **Developer-friendly** — fast to build, easy to maintain

---

## Frontend

| Tool | Version | Why |
|---|---|---|
| **React** | 18+ | Industry standard, large community, component-based |
| **Vite** | 5+ | Fastest dev build tool, replaces CRA |
| **Tailwind CSS** | 3+ | Utility-first, fast UI development, no custom CSS mess |
| **React Router** | 6+ | Client-side routing for SPA |
| **Zustand** | 4+ | Lightweight state management, simpler than Redux |
| **React Query** | 5+ | Server state, caching, background refetching |

---

## Backend & Database

| Tool | Version | Why |
|---|---|---|
| **Supabase** | Latest | Open source Firebase alternative — gives us Auth + Postgres DB + Realtime + Storage for free |
| **PostgreSQL** | 15+ | Via Supabase — robust relational DB for coupons, users, votes |
| **Supabase Auth** | Latest | Google/Email login out of the box, free up to 50,000 users |
| **Supabase Realtime** | Latest | Live feed updates without custom websocket code |

---

## Hosting & Deployment

| Tool | Why | Cost |
|---|---|---|
| **Vercel** | Best for React/Vite apps, auto deploys from GitHub, free tier generous | Free (MVP) |
| **Supabase** | Hosted Postgres + backend, free tier: 500MB DB, 2GB bandwidth | Free (MVP) |
| **Cloudflare** | DNS + CDN + DDoS protection | Free |

---

## AI & Agents

| Tool | Role | Cost |
|---|---|---|
| **Google Gemini API** | Deal extraction & categorization — parses raw text from Reddit/Telegram/brand pages into structured deal data | Free tier: 1,500 req/day |
| **Google Gemini Gems** | Custom DealDrop AI assistant — users can query deals in natural language ("best Swiggy deals today") | Free (built on Gemini) |
| **Claude API (Anthropic)** | Deal description writing, duplicate detection, quality validation, smart recommendations | Pay-per-use, ~$0.003/deal |
| **Reddit API** | Scrape r/frugalIndia, r/india for deal posts — AI agent processes and posts to feed | Free tier available |
| **Telegram Bot API** | Monitor Indian deal channels, extract and relay deals to DealDrop | Free |
| **Vercel Cron Jobs** | Schedule AI deal discovery agents to run hourly/daily | Free (MVP) |

---

## Affiliate & Deal Data

| Source | Type | Cost |
|---|---|---|
| **EarnKaro API** | Indian brand deals feed (Swiggy, Flipkart, MakeMyTrip etc.) | Free to join |
| **VCommission** | Affiliate network — travel, fashion, electronics | Free to join |
| **CueLinks** | Auto-monetizes any link + deal feed | Free to join |
| **Manual Curation** | Admin adds deals manually via Supabase dashboard | ₹0 |

---

## Security & Rate Limiting

| Tool | Why | Cost |
|---|---|---|
| **Supabase RLS (Row Level Security)** | Enforces per-user data access at DB level — users can only read/write their own coupons | Free (built-in) |
| **Vercel Edge Middleware** | Rate limiting on API routes — prevents coupon code scraping and vote manipulation | Free (MVP) |

---

## Dev Tools

| Tool | Why |
|---|---|
| **Claude Code** | AI coding assistant in terminal |
| **GitHub** | Version control + open source community |
| **VS Code** | Primary IDE |
| **Postman** | API testing |
| **Vercel Analytics** | Free web analytics |

---

## Database Schema (MVP)

```sql
-- Users
users (id, email, name, avatar_url, created_at)

-- Personal Vault
coupons (
  id, user_id, store_name, code, discount,
  category, tags[], expiry_date, notes,
  is_public, created_at
)

-- Community Feed
community_deals (
  id, user_id, store_name, code, discount,
  category, tags[], expiry_date, upvotes,
  downvotes, status (active/expired),
  affiliate_url, created_at
)

-- Votes
votes (id, user_id, deal_id, vote_type)

-- Comments
comments (id, user_id, deal_id, text, created_at)
```

---

## Architecture Diagram

```
[User Browser]
     │
     ▼
[Vercel CDN] ──→ [React + Vite App]
                      │
           ┌──────────┴──────────┐
           ▼                     ▼
    [Supabase Auth]      [Supabase Postgres]
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
            [Personal Vault]   [Community Feed]
                                        │
                                        ▼
                              [Affiliate APIs]
                          (EarnKaro / VCommission)
```

---

## Future Stack (V2/V3)

| Addition | When | Why |
|---|---|---|
| **React Native / Expo** | V3 | Mobile app |
| **Redis (Upstash)** | V2 | Caching affiliate API responses |
| **Resend** | V2 | Email notifications (expiry reminders) |
| **OneSignal** | V2 | Push notifications |
| **Stripe** | V3 | Coupon marketplace payments |
| **Next.js migration** | V3 | SEO — deal pages need to be crawlable |
