# GrabDash Cloudflare Template — Implementation Progress

_Last scanned: 2026-04-02_

## Phase 1: Foundation

- [x] `package.json` — plugin deps added (`plugin-forms`, `plugin-webhook-notifier`, `@astrojs/react`)
- [x] `astro.config.mjs` — plugins, sandbox, marketplace configured
- [x] `src/styles/theme.css` — exists (all vars commented out as override reference — by design)
- [x] `seed/seed.json` — exists (~31KB, complete schema)
- [x] `src/layouts/Base.astro` — exists (~27KB, unified layout)

## Phase 2: Blog Features

- [x] `src/utils/reading-time.ts` — copied from blog
- [x] `src/components/PostCard.astro` — copied from blog
- [x] `src/components/TagList.astro` — copied from blog
- [x] `src/pages/posts/index.astro` — adapted from blog
- [x] `src/pages/posts/[slug].astro` — adapted from blog
- [x] `src/pages/category/[slug].astro` — adapted from blog
- [x] `src/pages/tag/[slug].astro` — adapted from blog
- [x] `src/pages/search.astro` — adapted from blog (multi-collection)
- [x] `src/pages/rss.xml.ts` — adapted from blog

## Phase 3: Portfolio Features

- [x] `src/components/ProjectCard.astro` — copied from portfolio
- [x] `src/pages/work/index.astro` — adapted from portfolio
- [x] `src/pages/work/[slug].astro` — adapted from portfolio

## Phase 4: Marketing Features

- [x] `src/components/MarketingBlocks.astro` — copied from marketing
- [x] `src/components/blocks/Hero.astro` — copied from marketing
- [x] `src/components/blocks/Features.astro` — copied from marketing
- [x] `src/components/blocks/Testimonials.astro` — copied from marketing
- [x] `src/components/blocks/Pricing.astro` — copied from marketing
- [x] `src/components/blocks/FAQ.astro` — copied from marketing
- [x] `src/components/blocks/index.ts` — copied from marketing
- [x] `src/pages/pricing.astro` — adapted from marketing
- [x] `src/pages/contact.astro` — adapted from marketing

## Phase 5: Ecommerce Features

- [x] `src/utils/price.ts` — new
- [x] `src/components/ProductCard.astro` — new
- [x] `src/components/ProductGallery.astro` — new
- [x] `src/components/CartDrawer.tsx` — new (React island, ~8KB)
- [x] `src/pages/shop/index.astro` — new
- [x] `src/pages/shop/[slug].astro` — new
- [x] `src/pages/cart.astro` — new

## Phase 6: Shared Pages

- [x] `src/pages/index.astro` — home page
- [x] `src/pages/pages/[slug].astro` — generic page renderer
- [x] `src/pages/about.astro` — about page
- [x] `src/pages/404.astro` — error page

## Notes

- `src/pages/[slug].astro` exists at root (not in plan) — may be a stale catch-all, needs review
- `src/live.config.ts` exists (fixed in previous session — uses correct export pattern)

## Verification

- [ ] Dev server starts without errors (`pnpm dev`)
- [ ] Seed runs correctly via dev-bypass
- [ ] All pages render: /, /posts, /posts/[slug], /work, /work/[slug], /shop, /shop/[slug], /pricing, /contact, /search, /cart, /pages/about, /category/[slug], /tag/[slug]
- [ ] Cart: add products, count badge, drawer, quantity controls
- [ ] Theme switcher (light/dark/system)
- [ ] Search across posts, projects, products
- [ ] `pnpm build` succeeds
