# Sprint 4 — AI Deal Discovery Agents
**Goal:** Platform auto-discovers and posts deals without manual work.

---

## S14 — EarnKaro Affiliate Feed (~2h)
**Create:**
- `src/agents/earnkaro-agent.js` — fetch + map to schema
- `api/cron/sync-earnkaro.js` — Vercel serverless endpoint

**earnkaro-agent.js pattern:**
```js
import { supabase } from '../lib/supabase.js'

export async function syncEarnKaro() {
  const res = await fetch('https://api.earnkaro.com/v1/deals', {
    headers: { Authorization: `Bearer ${process.env.VITE_EARNKARO_API_KEY}` }
  })
  const deals = await res.json()

  for (const deal of deals) {
    // Check duplicate
    const { data: existing } = await supabase
      .from('community_deals')
      .select('id')
      .eq('code', deal.couponCode)
      .single()

    if (existing) continue

    await supabase.from('community_deals').insert({
      store_name: deal.brandName,
      code: deal.couponCode,
      discount: deal.discountText,
      category: mapCategory(deal.category),
      affiliate_url: deal.trackingUrl,
      source_url: deal.dealUrl,
      posted_by: 'ai',
      status: 'active',
    })
  }
}
```

**api/cron/sync-earnkaro.js:**
```js
import { syncEarnKaro } from '../../src/agents/earnkaro-agent.js'

export default async function handler(req, res) {
  if (req.headers['x-cron-secret'] !== process.env.CRON_SECRET)
    return res.status(401).end()
  await syncEarnKaro()
  res.status(200).json({ ok: true })
}
```

**Done when:**
- [ ] Fetches EarnKaro feed successfully
- [ ] De-duplicates by `code` before insert
- [ ] Each deal has `posted_by = 'ai'`, `affiliate_url` set
- [ ] Endpoint returns 401 without correct `CRON_SECRET`

---

## S15 — Gemini AI Deal Extraction (~3h)
**Create:** `src/agents/gemini-extractor.js`

**Prompt:**
```
Extract coupon/deal info from this text. Return ONLY valid JSON, no markdown:
{"store_name": "", "code": null, "discount": "", "category": "", "expiry_date": null, "description": ""}
Use null for unknown fields. category must be one of: Food, Travel, Shopping, Banking, Entertainment, Other.

Text: {input}
```

```js
import { GoogleGenerativeAI } from '@google/generative-ai'

const genAI = new GoogleGenerativeAI(process.env.VITE_GEMINI_API_KEY)
const model = genAI.getGenerativeModel({ model: 'gemini-1.5-flash' }) // cheapest + fast

export async function extractDeal(rawText) {
  const result = await model.generateContent(PROMPT.replace('{input}', rawText))
  const text = result.response.text()
  try {
    return JSON.parse(text)
  } catch {
    return null // skip unparseable responses
  }
}
```

**Install:** `npm install @google/generative-ai`

**Done when:**
- [ ] Returns structured JSON from unstructured deal text
- [ ] Returns `null` gracefully on parse failure (don't throw)
- [ ] Uses `gemini-1.5-flash` (not pro — stays within free tier limits)

---

## S16 — Reddit Scraper Agent (~2h)
**Create:** `src/agents/reddit-agent.js` + `api/cron/sync-reddit.js`

**Reddit OAuth:** POST to `https://www.reddit.com/api/v1/access_token` with client credentials, then GET `/r/frugalIndia/new.json?limit=25`

**Filter posts:** include if title contains any of: `off`, `%`, `coupon`, `code`, `deal`, `offer`, `free`

**Flow:**
```
fetch top 25 posts from r/frugalIndia (last 24h)
  → pass each post title + body through gemini-extractor.js
  → if result has store_name + discount → check duplicate → insert
```

**Also scrape:** `r/india` with flair filter for "Deals & Offers"

**Done when:**
- [ ] Fetches Reddit posts with OAuth (no user login needed — app-only)
- [ ] Each post through Gemini extractor
- [ ] Valid deals inserted, duplicates skipped
- [ ] `source_url` set to Reddit post URL

---

## S17 — Vercel Cron — Agent Runner (~1h)
**Create:** `vercel.json` in project root:
```json
{
  "crons": [
    { "path": "/api/cron/sync-earnkaro", "schedule": "0 * * * *" },
    { "path": "/api/cron/sync-reddit",   "schedule": "0 */6 * * *" }
  ]
}
```

**EarnKaro:** every hour · **Reddit:** every 6 hours (Reddit API rate limits)

**Add `CRON_SECRET` to Vercel env vars** — a random string; all cron handlers check for it.

**Done when:**
- [ ] `vercel.json` in place with both crons
- [ ] Each endpoint returns 200 + `{ inserted: N }` count
- [ ] `CRON_SECRET` check working (401 without it)
- [ ] Test manually: `curl -H "x-cron-secret: ..." https://dealdrop.in/api/cron/sync-earnkaro`
