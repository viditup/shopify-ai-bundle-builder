🚀 Shopify AI Bundle Builder
An AI-powered "Frequently Bought Together" / bundle-builder feature for a
gummies & sports-nutrition Shopify store, built with Shopify Liquid +
Sections, vanilla JavaScript (ES6+), the Shopify AJAX Cart API, and
a small Node.js/Express backend that calls Google Gemini and matches
the AI's suggestions against the store's real, in-stock product catalog.
---
📌 What it does
On a product page, the AI Recommended Bundle section:
Sends the viewed product's title/type/tags/description to the backend.
The backend asks Gemini for complementary product ideas
(e.g. Mass Gainer → Whey Protein, Creatine, Shaker Bottle, Multivitamin).
The backend matches those ideas against the store's real Shopify
catalog (via the Storefront API) so only products that actually exist
and are in stock are ever shown.
The theme renders the matched products as cards (image, title, price,
AI reason) with Add to Cart buttons, plus an Add Entire Bundle to
Cart button that adds everything via the AJAX Cart API in one request.
If nothing can be matched, the section falls back to a merchant-chosen
collection (or a friendly message) instead of showing nothing.
---
🏗️ Project structure
```
shopify-ai-bundle-builder/
├── server/                         # Backend (Node.js + Express + Gemini)
│   ├── controllers/aiController.js #   Orchestrates Gemini + Shopify matching + caching
│   ├── services/geminiService.js   #   Gemini prompt + response parsing
│   ├── services/shopifyService.js  #   Storefront API catalog fetch + matching
│   ├── routes/aiRoutes.js          #   /api/ai/* routes + rate limiting
│   ├── server.js                   #   Express app entrypoint
│   ├── .env.example                #   Required environment variables
│   └── package.json
│
├── shopify-theme/                  # Full Dawn-based theme with the feature wired in
│   ├── sections/ai-recommended-bundle.liquid   # The section (settings + markup)
│   ├── snippets/skeleton-loader.liquid         # Loading placeholder cards
│   ├── snippets/toast.liquid                   # Add-to-cart toast container
│   ├── assets/bundle.css                       # Section styles
│   ├── assets/bundle-api.js                    # Backend API client (fetch + timeout)
│   ├── assets/ai-bundle.js                     # Fetches + renders recommendations
│   ├── assets/bundle-cart.js                   # AJAX Cart API (single + bundle add)
│   ├── assets/analytics.js                     # Impression / click / add-to-cart tracking
│   └── templates/product.json                  # Section wired into the product page
│
├── package.json                    # `npm run tunnel` helper (ngrok) for local dev
└── README.md
```
---
⚙️ Setup instructions
1. Backend
```bash
cd server
npm install
cp .env.example .env
```
Fill in `.env`:
Variable	Description
`PORT`	Port to run the server on (default `3000`)
`ALLOWED_ORIGINS`	Comma-separated storefront origins allowed to call the API, or `*` for local dev
`GEMINI_API_KEY`	Your Google Gemini API key
`SHOPIFY_STORE_DOMAIN`	e.g. `your-store.myshopify.com`
`SHOPIFY_STOREFRONT_ACCESS_TOKEN`	Storefront API token (create a custom app in Shopify Admin → Apps → Develop apps, and enable Storefront API access)
`SHOPIFY_API_VERSION`	Defaults to `2024-10`
`CATALOG_CACHE_TTL` / `RECOMMENDATION_CACHE_TTL`	Cache lifetimes in seconds
`RATE_LIMIT_PER_MINUTE`	Requests/minute allowed per IP on the recommendations endpoint
Run it:
```bash
npm run dev     # nodemon, auto-restarts
# or
npm start
```
Expose it to the internet for your dev store (Shopify can't reach
`localhost`):
```bash
cd ..
npm install
npm run tunnel   # starts `ngrok http 3000`
```
Copy the `https://xxxx.ngrok-free.app` URL it prints.
2. Theme
```bash
cd shopify-theme
shopify theme dev --store your-store.myshopify.com
```
In the Theme Editor, open a product page, select the "AI Recommended
Bundle" section (already added to the product template) and set:
AI backend URL → the ngrok URL from step 1 (no trailing slash)
Section title, Number of recommendations (3–5), and a Fallback
collection to show if no AI match is found
Enable/disable the section entirely with the Enable checkbox
Push the theme when you're ready:
```bash
shopify theme push
```
---
🧠 AI integration workflow
```
Product page loads
   │
   ▼
ai-bundle.js reads product data attributes (title, type, tags, description)
   │
   ▼
POST {backend}/api/ai/recommendations  { productId, title, type, tags, ... }
   │
   ▼
aiController.js
   ├─ checks in-memory response cache (per product+count, ~15 min TTL)
   ├─ geminiService.js → Gemini returns generic suggestions
   │      { reason, bundle: [{ name, category, reason }, ...] }
   ├─ shopifyService.js → fetches/caches store catalog (Storefront API, ~10 min TTL)
   │      and scores each suggestion against real product titles/types/tags
   └─ returns only in-stock, real Shopify products (image, price, variantId)
        or { fallback: true } if nothing matched
   │
   ▼
ai-bundle.js renders cards / fallback message, caches the result in
sessionStorage for the rest of the browsing session
   │
   ▼
bundle-cart.js adds single items or the entire bundle via /cart/add.js
```
---
✅ Requirements coverage
Liquid & Sections – dedicated section with theme-editor settings
(enable, title, count, fallback collection, backend URL).
JavaScript (ES6+) – classes, `async/await`, `fetch`, `AbortController`,
custom events, no jQuery.
AJAX Cart API – `/cart/add.js` for both single items and the full
bundle (one request with multiple line items).
AI integration – Gemini generates ideas; ideas are matched to real
products server-side (never invented/fake products shown to shoppers).
Loading states – skeleton cards, never a blank section.
Error handling – network/API/timeout/empty-response/product-not-found
all show a friendly message instead of breaking the page.
Performance – server-side response + catalog caching (`node-cache`),
client-side `sessionStorage` caching, `defer`-loaded scripts, request
timeouts, rate limiting.
Bonus – basic recommendation analytics events (`analytics.js`),
and a fallback-collection setting for when no match is found.
---
📝 Assumptions made
No public Shopify App / OAuth flow was built for this 24-hour scope; the
backend is a standalone Node service the storefront calls directly (CORS
enabled, tunneled via ngrok for local dev). A production version would
move this behind a Shopify App Proxy so no public backend URL is
exposed in theme settings — noted as the natural next step.
Product matching uses lightweight token-overlap scoring against the
Storefront API catalog rather than a vector/embedding search, which is
fast, dependency-free, and accurate enough for a curated set of store
categories (Mass Gainers, Whey Protein, Creatine, etc.).
"Recommendation Analytics" (bonus) logs structured events to
`window.dataLayer`/console rather than shipping a full analytics backend.
Money is formatted client-side via `Intl.NumberFormat` using the
currency returned by the Storefront API, so it works regardless of the
shop's locale.
---
🎥 Demo video checklist
Product page → scroll to AI Recommended Bundle → skeleton loader → real recommendations appear.
Add a single product from the bundle → cart updates, toast confirms.
Add the entire bundle → all items added in one cart request.
Theme editor → change section title / recommendation count / fallback collection live.
Quick code walkthrough: `sections/ai-recommended-bundle.liquid` → `ai-bundle.js` → `server/controllers/aiController.js` → `shopifyService.js` matching logic.

<img width="1341" height="587" alt="image" src="https://github.com/user-attachments/assets/2a8a366d-e95e-422a-929f-6e3e4c7ac939" />

