# Sprint 2 — Personal Vault
**Goal:** Authenticated users can add, view, edit, and delete personal coupons.

---

## S05 — Add Coupon Form (~2h)
**Form fields:** Store Name (req), Code (req), Discount, Category (select), Tags (multi), Expiry Date, Notes, Make Public (toggle)

**Categories:** Food / Travel / Shopping / Banking / Entertainment / Other

**Create:**
- `src/components/vault/add-coupon-modal.jsx` — modal with form
- `src/hooks/use-coupons.js` — React Query hooks for coupon CRUD

**use-coupons.js pattern:**
```js
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query'
import { supabase } from '../lib/supabase'

export function useVaultCoupons(userId) {
  return useQuery({
    queryKey: ['coupons', userId],
    queryFn: () => supabase.from('coupons').select('*').eq('user_id', userId).order('expiry_date')
  })
}

export function useAddCoupon() {
  const qc = useQueryClient()
  return useMutation({
    mutationFn: (data) => supabase.from('coupons').insert(data),
    onSuccess: () => qc.invalidateQueries({ queryKey: ['coupons'] })
  })
}
```

**When "Make Public" is toggled on submit:** also insert into `community_deals` with `posted_by = 'user'`.

**Done when:**
- [ ] Form validates required fields
- [ ] Submits insert to `coupons` table
- [ ] Public toggle also inserts to `community_deals`
- [ ] Modal closes and vault refreshes
- [ ] Error toast on failure

---

## S06 — Coupon Card Component (~1h)
**Shows:** Store name + category icon · Code (click to copy) · Discount badge · Expiry indicator · Tags as chips

**Expiry logic in `src/lib/utils.js`:**
```js
export function getExpiryStatus(dateStr) {
  if (!dateStr) return 'none'
  const days = Math.ceil((new Date(dateStr) - new Date()) / 86400000)
  if (days < 0) return 'expired'
  if (days <= 7) return 'soon'
  return 'fresh'
}
```

**Status → styles:**
- `fresh` → green-500 badge "🟢 X days left"
- `soon` → yellow-500 badge "🟡 Expiring soon"
- `expired` → red-500 badge "🔴 Expired"

**Create:** `src/components/deals/coupon-card.jsx`

**Done when:**
- [ ] All 3 expiry states render correctly
- [ ] Click on code copies to clipboard + shows "Copied!" for 2s
- [ ] Full width on mobile, grid on desktop

---

## S07 — Personal Vault Page (~2h)
**Create:**
- `src/pages/vault.jsx`
- `src/components/vault/vault-filter-bar.jsx`

**Features:**
- Grid of `<CouponCard>` — sorted by expiry (soonest first)
- Filter chips: All / Food / Travel / Shopping / Banking / Expiring Soon
- Search input — client-side filter on `store_name + code + tags` (no extra queries)
- "Add Coupon" button → opens modal
- Empty state: illustration + "Add your first coupon" CTA

**Data:** Fetch only logged-in user's coupons via `useVaultCoupons(user.id)`

**Done when:**
- [ ] Only logged-in user's coupons shown
- [ ] Filter + search work client-side
- [ ] Empty state renders
- [ ] Sorted by expiry

---

## S08 — Edit & Delete Coupon (~1h)
**Create:** `src/components/vault/coupon-actions-menu.jsx` — three-dot menu on each card

**Edit:** opens `<AddCouponModal>` pre-filled with existing coupon data → on submit calls `UPDATE`

**Delete:** confirm dialog → `supabase.from('coupons').delete().eq('id', id)`

**Add to `use-coupons.js`:**
```js
export function useUpdateCoupon() { /* same pattern as useAddCoupon but .update() */ }
export function useDeleteCoupon() { /* .delete().eq('id', id) */ }
```

**Done when:**
- [ ] Edit pre-fills all fields
- [ ] Save updates Supabase row
- [ ] Delete prompts confirmation before removing
- [ ] Vault re-fetches after either action
