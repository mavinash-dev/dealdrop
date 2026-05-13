# Sprint 3 — Community Feed
**Goal:** Users browse, share, vote on, and validate community deals.

---

## S09 — Community Feed Page (~2h)
**Create:**
- `src/pages/community.jsx`
- `src/hooks/use-community-deals.js`

**Query:** `community_deals` where `status = 'active'`, 20 per page, sorted by `created_at` desc by default.

**Sort options:** Latest / Most Upvoted / Expiring Soon (toggle buttons above feed)

**Filter:** Category chips (same set as vault)

**AI vs User badge:**
- `posted_by === 'ai'` → show `🤖 AI` chip on card
- `posted_by === 'user'` → show user avatar

**Pagination:** "Load more" button — append next page to list (don't replace).

**Done when:**
- [ ] Fetches active deals from Supabase
- [ ] Sort + filter work (server-side query params)
- [ ] AI / user badges visible
- [ ] Load more appends correctly

---

## S10 — Share to Community (~1h)
**"Share a Deal" button** on community page → opens `<AddCouponModal>` with `is_public` forced `true` and no vault save.

**Also:** from vault, a "Make Public" action on existing private coupon copies it to `community_deals`.

**Done when:**
- [ ] Deal appears in community feed immediately after posting
- [ ] `posted_by` set to `'user'`
- [ ] No duplicate inserted into `coupons` table when sharing directly

---

## S11 — Upvote / Downvote (~1h)
**Create:** `src/components/deals/vote-buttons.jsx`

**Logic:**
1. Insert into `votes (user_id, deal_id, vote_type)`
2. Optimistically update `upvotes`/`downvotes` count on card
3. Same vote again → delete from `votes` (toggle off), decrement count
4. Unauthenticated → redirect to `/login`

**Use `useMutation` with `onMutate` for optimistic update + `onError` rollback.**

**Done when:**
- [ ] Counts update instantly (optimistic)
- [ ] Supabase confirms — rollback on error
- [ ] User's active vote highlighted (indigo for up, red for down)
- [ ] Unauthed users see counts but clicking prompts login

---

## S12 — Mark Working / Expired (~1h)
**Create:** `src/components/deals/deal-status-buttons.jsx` — ✅ Working | ❌ Expired

**Logic:**
- "Expired" click → increment `downvotes`; if `downvotes >= 5` → set `status = 'expired'`
- "Working" click → increment `upvotes`
- Expired deals: red overlay + "Reported as expired" label, hidden from default feed

**Done when:**
- [ ] Buttons visible on each community deal card
- [ ] Status updates in Supabase
- [ ] 5+ expired reports archives the deal (status = 'expired')
- [ ] Expired deals not shown in main feed

---

## S13 — Search & Filter (~2h)
**Create:** `src/pages/search.jsx`

**Behaviour:**
- Search input → debounced 300ms → Supabase `.ilike('store_name', '%q%')` OR `.ilike('code', '%q%')`
- URL updates with `?q=` param (use `useSearchParams` from react-router)
- Category filter chips below search bar
- "No results" state with "Share this deal" CTA

**Done when:**
- [ ] Search debounced, no query on every keystroke
- [ ] Results show store, discount, expiry, category
- [ ] `?q=` in URL — shareable link works
- [ ] No results state shown when empty
