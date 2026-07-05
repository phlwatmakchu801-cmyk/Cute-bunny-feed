# หนูน้อยกระต่ายนำโชค 🐰

Thai lottery formula web app with kawaii bunny theme — image feed, chat, formulas, spin wheel, premium subscriptions, and admin panel. All data stored in LocalStorage (no backend required).

## Run & Operate

- `pnpm --filter @workspace/bunny-app run dev` — run the bunny app (port 19332)
- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm run typecheck` — full typecheck across all packages

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite + Tailwind v4 + framer-motion + wouter + shadcn/ui
- Storage: 100% LocalStorage (no database)
- No backend API needed for core features

## Where things live

- `artifacts/bunny-app/src/lib/auth.ts` — all types (User, Post, ChatMessage, Video, LotteryResult, PremiumRequest) + CRUD helpers
- `artifacts/bunny-app/src/lib/storage.ts` — LocalStorage helpers + initializeApp()
- `artifacts/bunny-app/src/contexts/AuthContext.tsx` — auth state
- `artifacts/bunny-app/src/contexts/ThemeContext.tsx` — 5 color themes + dark mode
- `artifacts/bunny-app/src/pages/` — all pages
- `artifacts/bunny-app/src/components/` — BottomNav, Sidebar, PostCard, StickerPanel

## Architecture decisions

- All state in LocalStorage. Keys: `bunny_users`, `bunny_posts`, `bunny_global_chat`, `bunny_chat_{id1}_{id2}` (sorted), `bunny_results_{type}`, `bunny_premium_requests`, `bunny_videos`, `bunny_theme`, `bunny_dark_mode`, `bunny_daily_formula`
- Session enforcement: login generates `activeSession` token stored in both user object and localStorage. Mismatch = forced logout (single-device)
- Verification flow: register → /verify (upload 299 baht slip) → admin approves → access granted
- Premium: upload 99 baht/month slip → admin approves → premiumExpiry set to +30 days
- Formula results protected with `user-select: none` + `onContextMenu preventDefault`
- 5 color themes via `data-theme` on `document.documentElement`
- Dark mode via `.dark` class on `document.documentElement`

## Product

- **Login / Register / Forgot Password** — phone-based auth with single-device sessions
- **Verify Page** — upload PromptPay slip (299 baht) to unlock app
- **Home Feed** — posts with text/image/video, likes, saves, comments, YouTube embeds
- **Chat** — global room + private 1:1 chat with stickers and image sharing
- **Formulas** — free basic formula + premium (Lao, running numbers, daily)
- **Videos** — YouTube and uploaded video gallery with like/pin
- **Results** — Thai/Lao/Hanoi/stock lottery results with search/favorites
- **Profile** — avatar picker, name edit, password change
- **Settings** — 5 color themes + dark mode toggle
- **Premium** — 99 baht/month via PromptPay 0941465408
- **Admin Panel** — user management, slip approval, post moderation, add lottery results, edit daily formulas

## Credentials

- Admin login: phone `0000000000` / password `09414654088pui` (or name `แอดมิน 👑`)
- PromptPay: `0941465408` (verify: 299 baht, premium: 99 baht/month)

## User preferences

- Kawaii bunny theme with pink/pastel palette
- Thai language UI throughout
- Formula results are copy-protected (user-select: none)
- All data in LocalStorage — no external services needed

## Gotchas

- Always run `initializeApp()` on startup (done in AuthContext) to seed admin user and defaults
- Admin user id is `admin-001`, never delete it
- YouTube embed URLs: extract video ID with `getYouTubeId()` from auth.ts
- Private chat key = sorted user IDs joined with `_`
