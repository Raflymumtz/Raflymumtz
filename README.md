<!-- ===================== HERO ===================== -->

# Muhammad Rafly Mumtaz

**Software Engineer · Applied ML & Research** — B.Sc. Information Systems, Universitas Brawijaya
Malang, Indonesia

Full-stack web systems and applied machine learning research — from route-optimization algorithms and software-defect prediction to the bilingual, CMS-driven platform in this repository.

[GitHub](https://github.com/Raflymumtz) · [LinkedIn](https://www.linkedin.com/in/rafly-mumtaz-0714bb21a) · [Email](mailto:raflymumtaz23@gmail.com) · [Resume](public/assets/documents/cv-muhammad-rafly-mumtaz.pdf)

<!-- ===================== ABOUT ==================== -->

## About

Information Systems graduate (Universitas Brawijaya, 2024) working across full-stack engineering and applied ML research. Co-author of ten peer-reviewed papers (2024) on route-optimization algorithms, software-defect prediction, and image-compression effects on model accuracy, presented at ICITEE, ICOMIT, SIET, and ICTECA.

This repository is a working sample of that range: a bilingual (ID/EN), CMS-driven site with its own admin panel, Supabase-backed data layer, and an in-browser research-paper reader — built, documented, and maintained end to end.

## Current Focus

- Full-stack web platforms — Next.js, TypeScript, Supabase
- Applied machine learning & optimization algorithms
- Research-to-production workflows for published academic work
- Bilingual, accessible product design (ID/EN)

<!-- ================== SKILLS & TOOLKIT ================= -->

## Skills & Toolkit

<p align="center">
  <img src="https://skillicons.dev/icons?i=ts,go,next,react,tailwind,nodejs,supabase,postgres,git,github,figma" alt="Languages and tools" />
</p>

| Category | Stack |
|---|---|
| Languages | TypeScript, Go |
| Frontend | Next.js, React, Tailwind CSS, Framer Motion |
| Backend & Data | Node.js, Supabase (PostgreSQL, Auth, RLS) |
| Tooling | Git, GitHub, Figma |

<!-- ==================== RESEARCH ================== -->

## Research & Publications

Peer-reviewed papers (2024), co-authored during undergraduate study at Universitas Brawijaya — full PDFs in [`public/assets/papers/`](public/assets/papers/).

| Title | Venue | Focus |
|---|---|---|
| Exploring the Impact of Bayesian Optimization Towards Performance for Software Defect Prediction | ICTECA 2024 | Applied ML |
| Exploring the Impact of Ensemble Learning using Various AI Models for Software Defect Prediction | ICTECA 2024 | Applied ML |
| Performance Analysis of Route Segmentation Algorithm in Identifying Perimeter Area Around Travel Route | ICITEE 2024 | Algorithms |
| RouteSegmentation Algorithm to Identify Perimeter Area Surrounding Travel Path for Improved Navigation and POI Detection | ICOMIT 2024 | Algorithms |
| Performance Optimization of RouteSegmentation Algorithm Using Douglas-Peucker Line Simplification Approach | SIET 2024 | Algorithms |
| Optimizing Public Transit Route Recommendation Systems Using Time-based Dijkstra Algorithm: A Case Study | SIET 2024 | Algorithms |
| Exploring the Impact of Various Image Compression on Machine and Deep Learning Model Accuracy | SIET 2024 | Image Processing / ML |
| Performance Analysis of Lossy Image Formats with Huffman Encoding Across Different Resolutions | SIET 2024 | Image Processing |
| The Impact of Gamification Towards Repurchase Intention in E-Wallet Application | ICOMIT 2024 | Applied UX Research |
| The Impact of Utilitarian and Hedonic Gratification in Gamification to Continued Use Intentions of an E-Wallet Mobile Application | ICOMIT 2024 | Applied UX Research |

<!-- ==================== PHILOSOPHY ================== -->

## Engineering Philosophy

Trace every decision to a source — a file, a spec, or a stated tradeoff — rather than to convention alone.
Ship the working path before the ideal one, and mark clearly what's still a placeholder.
Prefer a data layer that degrades gracefully over one that fails loudly.

<!-- ==================== CONTACT =================== -->

## Connect

Open to conversations on full-stack engineering, applied ML, and research-driven product work.

[GitHub](https://github.com/Raflymumtz) · [LinkedIn](https://www.linkedin.com/in/rafly-mumtaz-0714bb21a) · [Email](mailto:raflymumtaz23@gmail.com)

---

<!-- ============================================================ -->
<!-- PROJECT DOCUMENTATION — unchanged from the original README   -->
<!-- ============================================================ -->

# porto-rafly

Muhammad Rafly Mumtaz's portfolio & services site — a bilingual (ID/EN), CMS-driven showcase for web/mobile/AI-ML development and published research. See `docs/DISCOVERY.md`, `docs/SITE_ARCHITECTURE.md`, and `docs/DESIGN_SYSTEM.md` for the full planning trail behind the decisions below.

## Tech stack

- **Framework**: Next.js 16 (App Router, Turbopack), TypeScript (strict), Tailwind CSS v4
- **i18n**: next-intl, locale-prefixed routing (`/id`, `/en`)
- **Motion**: Framer Motion + Lenis (momentum scrolling), gated by `prefers-reduced-motion` throughout
- **Paper reader**: react-pdf / pdf.js — research PDFs render inside the site, never handed off to the browser's PDF viewer
- **Backend**: Supabase (Postgres, Auth, Row Level Security) — see `supabase/migrations/`
- **Theming**: next-themes (dark default, light available), CSS custom properties as design tokens

## Architecture

```
src/
├── app/
│   ├── [locale]/       Public site (home, work, research, services, about, contact, search)
│   └── admin/          CMS — /admin/login (public) + /admin/(protected)/* (auth-gated)
├── components/         common/ layout/ home/ project/ research/ admin/ motion/
├── lib/
│   ├── supabase/       browser / server / service-role clients
│   ├── data/           data-access layer — queries Supabase, falls back to
│   │                   local fixtures when Supabase env vars aren't set,
│   │                   so the site runs before a database exists
│   └── validation/     zod schemas
├── i18n/                routing/navigation/request config for next-intl
├── messages/             id.json / en.json translation dictionaries
└── types/domain.ts       app-level content types
supabase/
├── migrations/          schema + RLS policies, in order
└── seed/seed.sql         taxonomy (categories/technologies) + the 5 services — no project/research content
scripts/import-content.ts Phase-8 content import (real papers + sample projects, all draft)
```

**Data layer contract**: every function in `lib/data/*` tries Supabase first and falls back to `lib/data/fixtures.ts` if `NEXT_PUBLIC_SUPABASE_URL`/`NEXT_PUBLIC_SUPABASE_ANON_KEY` aren't set. This is why `npm run dev` works immediately with no backend — but it also means **the site silently runs on fixture data until Supabase is actually connected**. Don't mistake a working `npm run dev` for a connected database.

## Local development

```bash
npm install
npm run dev
```

Runs at `http://localhost:3000`, redirecting to `/id` or `/en` based on browser language. Works with fixture data with no further setup.

## Connecting Supabase

1. Create a project at [supabase.com](https://supabase.com).
2. Copy `.env.example` to `.env.local` and fill in:
   - `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY` — Project Settings → API
   - `SUPABASE_SERVICE_ROLE_KEY` — same page, **server-only secret**, never commit it or prefix it `NEXT_PUBLIC_`
   - `NEXT_PUBLIC_SITE_URL` — your deployed URL (used for canonical/OG/sitemap)
3. Apply the schema, in order, via the Supabase SQL editor or CLI:
   ```bash
   # using the Supabase CLI, once `supabase link` is set up:
   supabase db push
   psql "$DATABASE_URL" -f supabase/seed/seed.sql
   ```
   Or paste each file under `supabase/migrations/` into the SQL editor in filename order, then `supabase/seed/seed.sql`.
4. Import the real starting content (author's own research papers + labeled sample projects, all as `draft`/`needs_review`):
   ```bash
   npm run import:content
   ```
5. Create the first admin user:
   - Supabase Dashboard → Authentication → Users → **Add user** (email + password).
   - SQL editor:
     ```sql
     insert into public.profiles (id, full_name, role)
     values ('<the new user''s UUID from the Users table>', 'Your Name', 'admin');
     ```
   - Sign in at `/admin/login`.

Everything imported or created via the CMS starts as `draft`/`needs_review` — nothing publishes itself. Review and publish through `/admin`.

## Admin CMS

**Where it lives: [`/admin`](http://localhost:3000/admin)** — unlinked from the public site by design and
`noindex`ed, so it's reachable only by typing the URL. It redirects to `/admin/login`
when signed out and `/admin/dashboard` when signed in. Note the admin routes sit
*outside* the `[locale]` segment: it's `/admin`, never `/id/admin`.

Signing in needs a connected Supabase project (see "Connecting Supabase" above) — auth is
Supabase Auth, so without it there is no account to sign in as. `/admin/login` says as much and
lists the missing env vars rather than just failing.

`/admin/login` → `/admin/dashboard`. Single `admin` role, no public sign-up path anywhere. Manage:

- **Projects** — bilingual case-study fields, technology tags, a modular block editor (text/heading/image/gallery/video/quote/metric/code/architecture/table/comparison/chart), and an optional AI/ML detail panel (dataset, model, metrics — only shown/rendered when actually filled in).
- **Research** — bilingual paper fields (abstract, method, results, etc.), matching the public `/research` editorial layout. **Paper PDF URL** is the file the in-site reader renders (a `/assets/papers/...` path or a public Storage URL); **External URL** stays the DOI/proceedings page.
- **Services** — the five service offerings (title/description/CTA per locale, active toggle).
- **Testimonials** — add/publish/delete; hidden site-wide until at least one exists.
- **Inquiries** — "Start a Project" form submissions.
- **Settings** — contact email & social links (empty by default, never invented), plus category/technology taxonomy.

## Reading papers in-site

`/research/[slug]` renders the paper itself further down the page: pdf.js rasterises each page to
a canvas inside the site's own chrome, with page navigation, zoom, fullscreen and download — the
reader never leaves for a browser PDF viewer or a new tab.

The pdf.js worker is copied out of `node_modules` into `public/pdf.worker.min.mjs` by
`scripts/copy-pdf-worker.mjs`, wired to `postinstall`. `pdfjs-dist` is pinned to the exact version
`react-pdf` depends on, because a worker/API version skew is what produces pdf.js's
"API version does not match Worker version" error. If you bump `react-pdf`, re-pin `pdfjs-dist` to
whatever it depends on and re-run `npm install`.

Which file gets rendered comes from `research.pdf_url` (set per paper in `/admin/research`), or
the fixture's `pdfUrl` when Supabase isn't connected.

## Production build

```bash
npm run lint
npm run typecheck
npm run build
```

All three must pass. `npm run build` prerenders every static-content route and needs no live Supabase connection (fixtures cover it), though real deployments should have Supabase connected before going live.

## Deployment

Target: Vercel (app) + Supabase (backend).

1. Push to the `porto-rafly` GitHub repo, import into Vercel.
2. Set the same env vars from `.env.local` in the Vercel project settings.
3. Set `NEXT_PUBLIC_SITE_URL` to the production domain.
4. Deploy. Confirm `/robots.txt` and `/sitemap.xml` resolve, and that `/admin/login` is reachable but not indexed (already set via `robots: { index: false }` in the admin layout).

## Known limitations / next steps

- No live Supabase project is connected yet in this environment — the data layer's fallback path is what's actually been exercised end-to-end.
- "Selected Works" ships with two clearly-labeled sample projects (`is_sample: true`) until real project content is supplied and imported.
- The block editor's less common block types (gallery/video/table/comparison/chart) are authored via a JSON field in the admin rather than a bespoke per-type form — functional, but the more common types (text/heading/image/quote/metric/code) have dedicated fields.
- Chart-type case-study blocks intentionally don't render yet — no real metrics exist to chart; wire this up (with the `dataviz` design guidance) once a project supplies real numbers.
- The 10 real research PDFs (`docs/DISCOVERY.md` §3c) are self-hosted under `public/assets/papers/` (~23MB total) and rendered in-page by the reader on `/research/[slug]`. This is fine for now but bloats the deployment — move them to Supabase Storage before going to production and point each paper's `pdf_url` at the storage URL instead.
