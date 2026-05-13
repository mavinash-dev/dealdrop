# Sprint 1 — Foundation
**Goal:** Project runs locally, Supabase connected, user can sign in with Google.

---

## S01 — Project Scaffold (~1h)
**Run in order:**
```bash
npm create vite@latest dealdrop -- --template react
cd dealdrop
npm install
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
npm install react-router-dom@6 zustand @tanstack/react-query @supabase/supabase-js lucide-react
```

**tailwind.config.js** — set content:
```js
content: ["./index.html", "./src/**/*.{js,jsx}"]
```

**src/index.css:**
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

**src/main.jsx:**
```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { BrowserRouter } from 'react-router-dom'
const queryClient = new QueryClient()
// wrap App with both providers
```

**.env.example:**
```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
VITE_GEMINI_API_KEY=
VITE_CLAUDE_API_KEY=
VITE_EARNKARO_API_KEY=
VITE_REDDIT_CLIENT_ID=
VITE_REDDIT_CLIENT_SECRET=
CRON_SECRET=
```

**Done when:**
- [ ] `npm run dev` starts on :5173 with no errors
- [ ] Tailwind applies (test `className="text-red-500"`)
- [ ] `.env` is in `.gitignore`

---

## S02 — Supabase Schema & Migrations (~1h)
**Manual:** Create project at supabase.com → copy URL + anon key to `.env`

**Create:** `supabase/migrations/001_initial_schema.sql`
```sql
create extension if not exists "uuid-ossp";

create table public.profiles (
  id uuid references auth.users on delete cascade primary key,
  email text, name text, avatar_url text, created_at timestamptz default now()
);

create table public.coupons (
  id uuid default uuid_generate_v4() primary key,
  user_id uuid references public.profiles(id) on delete cascade not null,
  store_name text not null, code text not null, discount text,
  category text, tags text[], expiry_date date, notes text,
  is_public boolean default false, created_at timestamptz default now()
);

create table public.community_deals (
  id uuid default uuid_generate_v4() primary key,
  user_id uuid references public.profiles(id) on delete set null,
  posted_by text default 'ai',
  store_name text not null, code text, discount text, description text,
  category text, tags text[], expiry_date date,
  upvotes int default 0, downvotes int default 0,
  status text default 'active',
  affiliate_url text, source_url text, created_at timestamptz default now()
);

create table public.votes (
  id uuid default uuid_generate_v4() primary key,
  user_id uuid references public.profiles(id) on delete cascade not null,
  deal_id uuid references public.community_deals(id) on delete cascade not null,
  vote_type text not null,
  unique(user_id, deal_id)
);

create table public.comments (
  id uuid default uuid_generate_v4() primary key,
  user_id uuid references public.profiles(id) on delete cascade not null,
  deal_id uuid references public.community_deals(id) on delete cascade not null,
  text text not null, created_at timestamptz default now()
);

-- RLS
alter table public.profiles enable row level security;
alter table public.coupons enable row level security;
alter table public.community_deals enable row level security;
alter table public.votes enable row level security;
alter table public.comments enable row level security;

create policy "own profile" on public.profiles for all using (auth.uid() = id);
create policy "own coupons" on public.coupons for all using (auth.uid() = user_id);
create policy "read public coupons" on public.coupons for select using (is_public = true);
create policy "read deals" on public.community_deals for select using (true);
create policy "insert deals" on public.community_deals for insert with check (auth.uid() is not null);
create policy "own votes" on public.votes for all using (auth.uid() = user_id);
create policy "read comments" on public.comments for select using (true);
create policy "insert comments" on public.comments for insert with check (auth.uid() is not null);
```

**Create:** `src/lib/supabase.js`
```js
import { createClient } from '@supabase/supabase-js'
export const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY
)
```

**Done when:**
- [ ] SQL executed in Supabase SQL editor — all 5 tables exist
- [ ] RLS enabled on all tables
- [ ] `supabase.js` exports without errors

---

## S03 — Google Auth (~1h)
**Manual (Avinash):** Supabase Dashboard → Auth → Providers → Google → enable → add OAuth credentials from console.cloud.google.com

**src/store/auth-store.js:**
```js
import { create } from 'zustand'
import { supabase } from '../lib/supabase'
export const useAuthStore = create((set) => ({
  user: null,
  loading: true,
  setUser: (user) => set({ user, loading: false }),
  signIn: () => supabase.auth.signInWithOAuth({ provider: 'google' }),
  signOut: () => supabase.auth.signOut(),
}))
```

**src/main.jsx** — add `onAuthStateChange` listener that calls `setUser` and upserts into `profiles` table.

**Create:**
- `src/hooks/use-auth.js` — returns `{ user, loading, signIn, signOut }` from store
- `src/components/auth/sign-in-button.jsx` — Google button using `signIn()`
- `src/components/auth/auth-guard.jsx` — redirects to `/login` if no user
- `src/pages/login.jsx` — centered card with `<SignInButton />`

**Done when:**
- [ ] Google OAuth popup opens on click
- [ ] User in Zustand store after sign-in
- [ ] Profile row created in `profiles` on first sign-in
- [ ] `/vault` redirects unauthenticated users to `/login`

---

## S04 — App Layout & Navigation (~2h)
**Routes:**
```
/           Home (community discovery)
/vault      Personal Vault (auth-guarded)
/community  Community Feed
/search     Search
/login      Login
```

**Design tokens — use exactly these:**
```
Primary:  indigo-600    Accent:  amber-400
Success:  green-500     Warning: yellow-500
Danger:   red-500       BG:      gray-50
Card:     white shadow-sm rounded-xl
```

**Create:**
- `src/components/layout/app-layout.jsx` — `<Header>` + `<Outlet>` + `<BottomNav>`
- `src/components/layout/header.jsx` — logo, nav links, avatar + sign-out (desktop)
- `src/components/layout/bottom-nav.jsx` — Home/Vault/Community/Search icons (mobile, `md:hidden`)
- `src/App.jsx` — final route config, wrap `/vault` with `<AuthGuard>`

**Done when:**
- [ ] All 5 routes render
- [ ] Header on desktop, bottom nav on mobile
- [ ] Active route highlighted
- [ ] `/vault` redirects if not authed
