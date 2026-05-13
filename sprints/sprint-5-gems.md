# Sprint 5 — Gemini Gems & Monetization
**Goal:** AI assistant live, affiliate links earning, duplicates auto-cleaned.

---

## S18 — Gemini Gems Chat Interface (~3h)
Users ask for deals in natural language → AI searches our DB and returns cards.

> *"Find me Zomato deals under ₹100 off"*  
> *"Best travel deals expiring this week"*

**Create:**
- `src/agents/gems-agent.js` — Gemini call with DB context
- `src/components/ai/gems-chat.jsx` — floating widget + drawer

**gems-agent.js flow:**
1. Receive user message
2. Query Supabase for relevant deals (keyword match on store_name, category, tags)
3. Pass deals + message to Gemini: *"User asks: {message}. Here are available deals: {JSON}. Return top 3–5 most relevant deal IDs and a one-line explanation."*
4. Return matched deals to UI

```js
export async function queryGems(userMessage) {
  // 1. Broad keyword fetch from Supabase (max 50 deals for context)
  const keyword = userMessage.split(' ').filter(w => w.length > 3).join(' | ')
  const { data: deals } = await supabase
    .from('community_deals')
    .select('id, store_name, discount, category, expiry_date, affiliate_url')
    .eq('status', 'active')
    .textSearch('store_name', keyword)
    .limit(50)

  // 2. Gemini picks best matches
  const prompt = `User query: "${userMessage}"\nDeals: ${JSON.stringify(deals)}\nReturn JSON: { "ids": [], "note": "" }`
  const result = await model.generateContent(prompt)
  const { ids, note } = JSON.parse(result.response.text())

  return { deals: deals.filter(d => ids.includes(d.id)), note }
}
```

**gems-chat.jsx:**
- Bottom-right floating button (💬)
- Opens slide-over drawer (right side, `w-80`)
- Input at bottom, results render as `<CouponCard>` inside drawer
- Max 10 messages in history (don't accumulate context forever)

**Done when:**
- [ ] Chat widget opens/closes
- [ ] Query returns 3–5 relevant deals from DB
- [ ] Results render as `CouponCard` components
- [ ] Handles zero results gracefully ("No deals found for that — try a different search")

---

## S19 — Affiliate Link Auto-Embed (~1h)
Every deal card gets a "Shop Now" CTA linking to `affiliate_url`.

**For deals without an affiliate_url** — wrap the `source_url` through CueLinks:
```js
// src/lib/affiliate.js
export function wrapWithCueLinks(url) {
  if (!url) return null
  return `https://linker.cuelinks.com/?url=${encodeURIComponent(url)}`
}
```

**Update `<CouponCard>`:**
- Add "Shop Now →" button at bottom of card
- Uses `deal.affiliate_url ?? wrapWithCueLinks(deal.source_url)`
- Opens in new tab

**Done when:**
- [ ] "Shop Now" button on every community deal card
- [ ] Correct affiliate URL used (earnkaro > cuelinks fallback)
- [ ] Opens in new tab (`target="_blank" rel="noopener"`)

---

## S20 — AI Duplicate Detection (~2h)
Before inserting any deal (agent or user), check for near-duplicates using Claude API.

**Create:** `src/agents/claude-dedup.js`

```js
import Anthropic from '@anthropic-ai/sdk'
const client = new Anthropic({ apiKey: process.env.VITE_CLAUDE_API_KEY })

export async function isDuplicate(newDeal, existingDeals) {
  if (!existingDeals.length) return { duplicate: false }

  const prompt = `Is this new deal a duplicate of any existing deal?
New: ${JSON.stringify(newDeal)}
Existing: ${JSON.stringify(existingDeals)}
Return ONLY JSON: { "duplicate": true/false, "matchedId": "uuid or null" }`

  const msg = await client.messages.create({
    model: 'claude-haiku-4-5-20251001', // cheapest — this runs on every insert
    max_tokens: 100,
    messages: [{ role: 'user', content: prompt }]
  })

  return JSON.parse(msg.content[0].text)
}
```

**Wire into agents:** Before any `supabase.from('community_deals').insert(...)`, call:
1. Fetch existing deals for same `store_name` from last 7 days
2. Pass to `isDuplicate()`
3. If `duplicate: true` → skip insert, optionally upvote `matchedId`

**Use `claude-haiku-4-5-20251001`** — not Sonnet/Opus. This runs on every deal insert so cost matters.

**Done when:**
- [ ] Exact code duplicates always blocked (this is also the Supabase unique check fallback)
- [ ] Near-duplicates (same store + similar discount) caught by Claude
- [ ] No visible duplicates in community feed after running agents
