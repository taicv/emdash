# GrabDash Cloudflare Template - Implementation Plan

## Context

The `grabdash-cloudflare` template needs to combine ALL features from three existing EmDash templates (blog-cloudflare, marketing-cloudflare, portfolio-cloudflare) and add WooCommerce-inspired ecommerce capabilities. Currently it's a minimal starter with only posts + pages. The goal is an "everything" template that demonstrates EmDash's full power.

## Architecture Decisions

- **Design system**: Blog-cloudflare's CSS variables as base, extended with portfolio's `--font-serif` (Playfair Display) and marketing's extra variables (`--color-primary`, `--shadow-xl`, `--radius-full`, etc.), plus ecommerce variables (`--color-sale`)
- **Navigation**: Blog layout (sticky glassmorphism nav) with expanded menu: Home, Blog, Work, Shop, Pricing, Contact + cart icon + search across all 4 collections
- **Taxonomies**: `category` + `tag` shared across posts & projects with distinct terms; `product_category` + `product_tag` separate for products
- **Home page**: Marketing hero block → Latest Posts → Selected Work → Featured Products
- **Cart**: React island (`client:load`) with localStorage. Custom DOM events for add-to-cart from server-rendered product pages
- **Generic pages**: At `/pages/[slug]` to avoid catch-all conflicts
- **Plugins**: forms + webhook-notifier (from blog template)

## Implementation Phases

### Phase 1: Foundation

| File                     | Action                                                                                                                                                                 |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `package.json`           | Add `@emdash-cms/plugin-forms`, `@emdash-cms/plugin-webhook-notifier` deps                                                                                             |
| `astro.config.mjs`       | Add formsPlugin, webhookNotifierPlugin, sandbox config, marketplace                                                                                                    |
| `src/styles/theme.css`   | Unified design system (blog base + serif font + marketing vars + ecommerce vars)                                                                                       |
| `seed/seed.json`         | Complete schema: 4 collections (posts, pages, projects, products), 4 taxonomies, menus, widgets, bylines, sections, all demo content                                   |
| `src/layouts/Base.astro` | Unified layout: blog's header/footer + Google Fonts (Inter, Playfair Display, JetBrains Mono) + Phosphor Icons CDN + cart icon slot + expanded search + theme switcher |

**seed.json collections:**

- `posts`: title, featured_image, content, excerpt (commentsEnabled)
- `pages`: title, content (portableText with marketing block support)
- `projects`: title, featured_image, client, year, summary, content, gallery(json), url
- `products`: title, featured_image, price(number), sale_price(number), sku, description(portableText), gallery(json), stock_status, weight

**seed.json content:**

- 7 blog posts (from blog-cloudflare seed) + 1 draft
- 3 pages: about, home (marketing.hero + marketing.features), pricing (marketing.pricing + marketing.faq)
- 4 projects (from portfolio-cloudflare seed)
- 6 products (new: leather tote, headphones, desk lamp, t-shirt, pour-over set, notebook)

### Phase 2: Blog Features (adapt from blog-cloudflare)

| File                              | Source                                                         |
| --------------------------------- | -------------------------------------------------------------- |
| `src/utils/reading-time.ts`       | Copy from blog                                                 |
| `src/components/PostCard.astro`   | Copy from blog                                                 |
| `src/components/TagList.astro`    | Copy from blog                                                 |
| `src/pages/posts/index.astro`     | Adapt from blog                                                |
| `src/pages/posts/[slug].astro`    | Adapt from blog (3-col, TOC, comments, widgets)                |
| `src/pages/category/[slug].astro` | Adapt from blog                                                |
| `src/pages/tag/[slug].astro`      | Adapt from blog                                                |
| `src/pages/search.astro`          | Adapt from blog (expand to search posts + projects + products) |
| `src/pages/rss.xml.ts`            | Adapt from blog                                                |

### Phase 3: Portfolio Features (adapt from portfolio-cloudflare)

| File                               | Source                                           |
| ---------------------------------- | ------------------------------------------------ |
| `src/components/ProjectCard.astro` | Copy from portfolio                              |
| `src/pages/work/index.astro`       | Adapt from portfolio (tag filtering, 2-col grid) |
| `src/pages/work/[slug].astro`      | Adapt from portfolio (gallery, "More Work")      |

### Phase 4: Marketing Features (adapt from marketing-cloudflare)

| File                                       | Source                               |
| ------------------------------------------ | ------------------------------------ |
| `src/components/MarketingBlocks.astro`     | Copy from marketing                  |
| `src/components/blocks/Hero.astro`         | Copy from marketing                  |
| `src/components/blocks/Features.astro`     | Copy from marketing                  |
| `src/components/blocks/Testimonials.astro` | Copy from marketing                  |
| `src/components/blocks/Pricing.astro`      | Copy from marketing                  |
| `src/components/blocks/FAQ.astro`          | Copy from marketing                  |
| `src/components/blocks/index.ts`           | Copy from marketing                  |
| `src/pages/pricing.astro`                  | Adapt from marketing                 |
| `src/pages/contact.astro`                  | Adapt from marketing (form handling) |

### Phase 5: Ecommerce Features (new)

| File                                  | Description                                                                                                      |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `src/utils/price.ts`                  | formatPrice(), getSalePercentage()                                                                               |
| `src/components/ProductCard.astro`    | Product card: square image, price/sale display, sale badge, stock status                                         |
| `src/components/ProductGallery.astro` | Main image + thumbnail row, JS thumbnail switching                                                               |
| `src/components/CartDrawer.tsx`       | React island: cart icon with badge + slide-out drawer with item list, quantity controls, subtotal, demo checkout |
| `src/pages/shop/index.astro`          | Product listing: sort by price/date, filter by product_category                                                  |
| `src/pages/shop/[slug].astro`         | Single product: gallery, price, description, add-to-cart, related products                                       |
| `src/pages/cart.astro`                | Full cart page (React island for cart contents)                                                                  |

**Cart architecture:**

- `CartDrawer.tsx` exports `<CartProvider client:load />` used in Base.astro
- CartProvider renders: nav icon with count badge + slide-out drawer
- localStorage key: `grabdash-cart` (JSON array of CartItem)
- Add-to-cart: product pages render `<button data-product='{...}'>` + inline `<script>` dispatching `cart:add` CustomEvent
- CartDrawer listens for `cart:add` events via useEffect

### Phase 6: Shared Pages

| File                           | Description                                                                      |
| ------------------------------ | -------------------------------------------------------------------------------- |
| `src/pages/index.astro`        | Home: MarketingBlocks hero → PostCard grid → ProjectCard grid → ProductCard grid |
| `src/pages/pages/[slug].astro` | Generic page renderer                                                            |
| `src/pages/about.astro`        | About page with sidebar (services, contact info) from portfolio pattern          |
| `src/pages/404.astro`          | Error page                                                                       |

## Verification

1. `pnpm install` in template dir
2. `npx emdash dev` starts without errors
3. Navigate to `/_emdash/api/setup/dev-bypass?redirect=/_emdash/admin` to seed
4. Verify all pages render: /, /posts, /posts/[slug], /work, /work/[slug], /shop, /shop/[slug], /pricing, /contact, /search, /cart, /pages/about, /category/[slug], /tag/[slug]
5. Verify cart: add products, see count badge, open drawer, change quantities, clear cart
6. Verify theme switcher (light/dark/system)
7. Verify search works across posts, projects, products
8. `pnpm build` succeeds
