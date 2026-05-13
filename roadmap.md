# 🗺️ DealDrop — Product Roadmap
**Version:** 1.0
**Date:** May 13, 2026
**Author:** Avinash (mavinash-dev)

---

## Vision

> To become India's most trusted community coupon platform — where every deal is real, every saving is effortless, and no one ever misses a discount again.

---

## MVP — v1.0 (Month 1)
**Theme: Build & Launch**
**Goal: Working product, first 100 users**

### Features
- [ ] User authentication (Supabase Auth — Google Sign-In)
- [ ] Personal coupon vault (add, edit, delete coupons)
- [ ] Expiry tracking — 🟢 Fresh / 🟡 Expiring Soon / 🔴 Expired
- [ ] Tags & categories (food, travel, shopping, bank, app-only, birthday)
- [ ] Community deal feed (share publicly)
- [ ] Upvote / downvote deals
- [ ] Mark deal as ✅ Working or ❌ Expired
- [ ] Search & filter (by store, category, tag)
- [ ] **AI Deal Discovery Agent** — Gemini API auto-discovers deals from Reddit, affiliate feeds, brand pages (runs on Vercel Cron, hourly)
- [ ] **AI Deal Categorization** — auto-tags store, category, discount value, expiry
- [ ] **Gemini Gems integration** — natural language deal search ("best Zomato deals today")
- [ ] Affiliate links embedded on all AI-discovered deals (earn from day 1)
- [ ] Responsive web app (mobile-friendly)
- [ ] Deploy on Vercel

### Milestones
- Week 1: Auth + core UI + personal vault
- Week 2: Community feed + voting + AI discovery agent
- Week 3: Gemini Gems assistant + affiliate links
- Week 4: Polish + deploy + launch

---

## v1.1 (Month 2)
**Theme: Fix & Grow**
**Goal: 500 users, first affiliate earnings**

- [ ] Bug fixes from user feedback
- [ ] User profiles (saved deals, contributed deals)
- [ ] Share deal via WhatsApp / link
- [ ] Report a deal button
- [ ] Store pages (all deals for one store)
- [ ] SEO basics (meta tags, OG images)
- [ ] Google Analytics integration
- [ ] Product Hunt launch

---

## v2.0 (Month 3–4)
**Theme: Engage & Retain**
**Goal: 1,000 DAU, ₹5,000/month affiliate revenue**

- [ ] Expiry reminder notifications (browser push via OneSignal)
- [ ] Email digest — "5 deals expiring this week" (Resend)
- [ ] Trending deals section (most upvoted this week)
- [ ] City-specific deals (Hyderabad, Bangalore, Mumbai)
- [ ] Bank & credit card offer filter
- [ ] Deal alert — "Notify me when [Store] has new deals"
- [ ] **AI Duplicate Detection** — Claude API flags and merges duplicate coupon codes
- [ ] **AI Deal Validation** — auto-archives deals with high expired-report rates; cross-checks source URL
- [ ] **Personalized AI feed** — Claude recommends deals based on user's vault history and browsing
- [ ] Leaderboard — top deal contributors of the month
- [ ] PWA support (installable on mobile home screen)
- [ ] Performance optimizations + Redis caching (Upstash)

---

## v2.5 (Month 5–6)
**Theme: Monetize**
**Goal: ₹25,000/month revenue, brand partnerships**

- [ ] Featured deals (brands pay for placement)
- [ ] Brand/merchant self-serve portal (post your own deals)
- [ ] Premium membership — ₹99/month (exclusive deals, no ads, early access)
- [ ] Deal bundles (e.g. "Best Hyderabad Food Deals This Week")
- [ ] Newsletter (weekly curated deals)
- [ ] Affiliate dashboard (track your clicks & earnings)
- [ ] API for deal data (for developers/partners)

---

## v3.0 (Month 7–9)
**Theme: Marketplace**
**Goal: ₹1,00,000/month revenue**

- [ ] Coupon Marketplace — users list & sell gift cards, referral codes, exclusive deals
- [ ] Secure transaction system (Stripe / Razorpay)
- [ ] Seller verification & reputation system
- [ ] Buyer protection policy
- [ ] Deal categories: Gift Cards, Referral Codes, Exclusive Codes, Subscription Deals
- [ ] Escrow system for high-value deals

---

## v4.0 (Month 10–12)
**Theme: Scale**
**Goal: Series A ready — aspirational target of 1M users (actual number depends on growth rate; focus on strong unit economics and engagement, not vanity metrics)**

- [ ] Native mobile app — React Native / Expo (iOS + Android)
- [ ] Browser extension — auto-detect & save coupons while browsing
- [ ] **AI Screenshot OCR** — user shares a screenshot of an SMS/UPI coupon, AI extracts and saves it to vault automatically
- [ ] **Agentic deal negotiation** — AI monitors price history and alerts user when a deal hits their target discount
- [ ] **AI-powered B2B API** — sell structured, AI-curated deal data to fintech apps, neobanks, super-apps
- [ ] White-label AI deal feed for banks (HDFC, ICICI can embed DealDrop agent)
- [ ] Migrate to Next.js for full SSR/SEO
- [ ] Multi-language support — AI translates deals into Hindi, Telugu, Tamil automatically

---

## Long-Term Vision (Year 2+)

| Feature | Impact |
|---|---|
| Integration with UPI apps | Auto-save coupons from GPay/PhonePe |
| Bank partnership API | Show relevant offers inside banking apps |
| Bharat expansion | Tier 2/3 cities, regional language support |
| Southeast Asia expansion | Similar coupon fragmentation problem in SG, MY, ID |
| Acquisition target | Paytm, PhonePe, or a fintech super-app acquires DealDrop |

---

## Version Summary

| Version | PRD Phase | Timeline | Theme | Key Metric |
|---|---|---|---|---|
| v1.0 | MVP | Month 1 | Launch | 100 users |
| v1.1 | V2 | Month 2 | Fix & Grow | 500 users |
| v2.0 | V2 | Month 3–4 | Engage | 1K DAU |
| v2.5 | V3 | Month 5–6 | Monetize | ₹25K/month |
| v3.0 | V3 | Month 7–9 | Marketplace | ₹1L/month |
| v4.0 | Future | Month 10–12 | Scale | Series A ready |
