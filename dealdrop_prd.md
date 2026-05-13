# 🏷️ DealDrop — Product Requirements Document (PRD)
**Version:** 1.0  
**Date:** May 13, 2026  
**Author:** Avinash (mavinash-dev)  
**Approver:** Avinash  
**Status:** 🟡 Pending Approval

---

## 1. Executive Summary

DealDrop is a community-driven, personal coupon vault and deal discovery platform built for Indian consumers. It solves the fragmented coupon problem — where deals are scattered across UPI apps, food delivery apps, SMS, emails, and websites — by giving users one place to store, discover, share, and act on deals before they expire.

**Tagline:** *"Every deal. One place. Always fresh."*

---

## 2. Problem Statement

### The Pain Points
| Pain Point | Current Reality |
|---|---|
| Coupons are scattered | UPI apps, food apps, SMS, email, browser extensions — no single source |
| Deals expire unnoticed | No reminders, users forget coupons they already have |
| Existing coupon sites are stale | CouponDunia, GrabOn show outdated or non-working codes |
| App-specific & user-specific deals | Can't be found on any public platform |
| No community validation | No way to know if a coupon actually works today |
| No personal wallet | No app lets you store YOUR own received coupons |

### Who Faces This?
Every smartphone user in India who uses:
- Swiggy, Zomato, Blinkit
- GPay, PhonePe, Paytm
- Amazon, Flipkart, Meesho
- MakeMyTrip, Cleartrip, IRCTC
- Any bank or credit card offer

---

## 3. Vision & Goals

### Vision
To become India's most trusted and active coupon community — where every deal is real, every coupon is fresh, and no one ever misses a saving again.

### Goals for MVP (0–3 months)
- [ ] User authentication (Supabase Auth — Google Sign-In) — required for personal vault
- [ ] Launch personal coupon vault — users save their own coupons
- [ ] Launch community feed — users share & validate deals
- [ ] Basic search & filter by store/category/tag
- [ ] Expiry tracking with visual indicators
- [ ] Web app live and shareable

### Goals for V2 (3–6 months)
- [ ] Push/email expiry reminders
- [ ] Affiliate link integration (EarnKaro / VCommission)
- [ ] Store-specific deal pages
- [ ] Mobile PWA support
- [ ] User profiles (saved deals, contributed deals history)

### Goals for V3 (6–12 months)
- [ ] Coupon Marketplace — users sell gift cards & exclusive codes
- [ ] Brand/merchant dashboard — brands post verified deals
- [ ] Premium membership tier
- [ ] Native mobile app (React Native)

---

## 4. Target Users

### Primary User — "The Smart Saver"
- Age: 18–35
- Location: Tier 1 & Tier 2 Indian cities
- Behavior: Uses 3–5 apps daily, gets coupons via SMS/notifications, forgets to use them
- Motivation: Save money on everyday purchases (food, travel, shopping)

### Secondary User — "The Deal Hunter"
- Actively looks for deals before purchasing
- Shares working codes with friends on WhatsApp groups
- Will become the core community contributor

### Future User — "The Seller"
- Has unused gift cards, referral credits, or exclusive codes
- Wants to monetize them via the marketplace

---

## 5. Core Features — MVP

### 5.0 Authentication
- Google Sign-In via Supabase Auth (one-click, no password)
- Required for personal vault — without auth, coupons can't persist across devices or browser clears
- Anonymous browsing allowed for community feed discovery; vault requires login

### 5.1 Personal Coupon Vault
- Add coupon manually: store name, code, discount, expiry date, category, notes
- Edit / delete coupons
- Tag system: `food`, `travel`, `shopping`, `bank`, `app-only`, `birthday`
- Expiry status: 🟢 Fresh (>7 days) / 🟡 Expiring Soon (≤7 days) / 🔴 Expired
- Search & filter within personal vault

### 5.2 Community Deal Feed
- Users share coupons publicly to the feed
- Upvote / downvote system
- Mark as ✅ Working or ❌ Expired
- Comment: "Worked for me on Swiggy today!"
- Sort by: Latest / Most Upvoted / Expiring Soon
- Filter by: Category / Store / Tag

### 5.3 AI-Powered Deal Discovery
- **AI Deal Agent** runs on a schedule (daily/hourly) to auto-discover and post deals from:
  - EarnKaro / VCommission / CueLinks affiliate API feeds
  - Reddit (r/frugalIndia, r/india) via Reddit API
  - Brand offer pages (Swiggy, Zomato, Amazon, Flipkart) via web scraping
  - Telegram deal channels via Telegram Bot API
- **Google Gemini API** extracts: store name, code, discount value, expiry date, category from raw deal text
- **Claude API** writes clean descriptions, validates deal quality, and flags duplicates
- **Gemini Gems** — custom DealDrop AI assistant users can query: *"Find me Zomato deals under ₹100 off"*
- Affiliate links auto-embedded on all discovered deals — earns commission from day 1
- Admin override: Avinash can pin, edit, or remove any AI-posted deal
- Categories: Food, Travel, Fashion, Electronics, Entertainment, Banking

### 5.4 AI Deal Validation
- When a user marks a deal ❌ Expired, AI cross-checks against the source URL to confirm
- AI tracks upvote/downvote ratio and auto-archives deals with low trust scores
- Duplicate detection — AI prevents the same coupon code from being posted multiple times

### 5.5 Search
- Search across community feed + discovery section
- Auto-suggest store names
- Filter by category, expiry, store

---

## 6. Out of Scope for MVP
- Full user profiles & social features (Phase 2)
- Push/email expiry reminders (Phase 2)
- Coupon marketplace / selling (Phase 3)
- Mobile native app (Phase 3)
- Browser extension (Future)
- SMS/email coupon auto-import (Future)

---

## 7. Success Metrics

| Metric | MVP Target (Month 1) | Month 3 Target |
|---|---|---|
| Total users | 100 | 1,000 |
| Coupons saved (personal) | 200 | 2,000 |
| Community deals shared | 50 | 500 |
| Affiliate click-throughs | 20 | 300 |
| Daily active users | 10 | 100 |

---

## 8. User Stories

| As a... | I want to... | So that... |
|---|---|---|
| User | Save a coupon I got on Swiggy | I don't forget it before it expires |
| User | See community-shared Zomato deals | I can use a code I didn't have |
| User | Filter deals by "travel" | I find air ticket discounts quickly |
| User | Mark a coupon as expired | Others don't waste time on dead codes |
| User | Get an expiry reminder | I use my coupon before it's gone |
| Admin (Avinash) | Add curated deals manually | The discovery section always has fresh content |

---

## 9. Risks & Mitigations

| Risk | Likelihood | Mitigation |
|---|---|---|
| Community doesn't contribute deals | Medium | Seed with 50+ deals manually at launch |
| Affiliate API stops working | Low | Multiple affiliate sources, manual fallback |
| Copied by large player | Medium | Build community moat — network effects |
| Low traffic at launch | High | Product Hunt launch + WhatsApp/Reddit India seeding |
| Coupon codes get misused | Low | Rate limiting + report button |

---

## 10. Version Mapping

| PRD Phase | Roadmap Version | Timeline |
|---|---|---|
| MVP | v1.0 | Month 1 |
| V2 | v1.1 + v2.0 | Month 2–4 |
| V3 | v2.5 + v3.0 | Month 5–9 |
| Future | v4.0 | Month 10–12 |

---

## 11. Approval

| Role | Name | Status | Date |
|---|---|---|---|
| Product Owner & Approver | Avinash (mavinash-dev) | 🟡 Pending | May 13, 2026 |

---

*Once approved, Dev Docs (Technical Architecture, Cost Breakdown, GTM Strategy) will be generated.*
