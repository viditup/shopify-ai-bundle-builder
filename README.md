# 🚀 Shopify AI Bundle Builder

<p align="center">
  <b>AI-powered Frequently Bought Together & Smart Bundle Recommendation System for Shopify Stores</b>
</p>

<p align="center">
  Built with Shopify Liquid, JavaScript (ES6+), Shopify AJAX Cart API, Node.js, Express, and Google Gemini AI.
</p>

---

## ✨ Overview

**Shopify AI Bundle Builder** is an intelligent product recommendation system designed for **sports nutrition and supplement stores**.

It creates personalized bundles by analyzing the customer's currently viewed product and suggesting relevant complementary products using **Google Gemini AI**.

Unlike simple recommendation systems, this solution verifies AI suggestions against the store's **real Shopify catalog**, ensuring customers only see products that actually exist and are available.

---

# 🎯 Key Features

## 🤖 AI-Powered Recommendations

* Sends product information to Gemini AI:

  * Product title
  * Product type
  * Tags
  * Description

* Generates complementary product suggestions.

Example:

**Mass Gainer →**

* Whey Protein
* Creatine
* Multivitamin
* Pre-Workout

---

## 🛒 Smart Bundle Builder

Customers can:

✅ View AI recommended products
✅ Add individual products to cart
✅ Add the complete bundle with one click
✅ See product images, prices, and AI reasoning

---

## 🔍 Real Shopify Product Matching

AI never displays fake products.

The backend:

1. Gets AI recommendations
2. Fetches the Shopify catalog
3. Matches suggestions with real products
4. Checks product availability
5. Returns only valid products

---

# 🏗️ Architecture

```
Customer Product Page
          |
          ↓
Shopify Liquid Section
          |
          ↓
JavaScript Recommendation Engine
          |
          ↓
Node.js + Express API
          |
          ↓
Google Gemini AI
          |
          ↓
Shopify Storefront API Matching
          |
          ↓
AI Bundle Display + Cart
```

---

# 📂 Project Structure

```
shopify-ai-bundle-builder/

│
├── server/
│
│   ├── controllers/
│   │      └── aiController.js
│   │
│   ├── services/
│   │      ├── geminiService.js
│   │      └── shopifyService.js
│   │
│   ├── routes/
│   │      └── aiRoutes.js
│   │
│   ├── server.js
│   ├── .env.example
│   └── package.json
│
│
├── shopify-theme/
│
│   ├── sections/
│   │      └── ai-recommended-bundle.liquid
│   │
│   ├── assets/
│   │      ├── ai-bundle.js
│   │      ├── bundle-cart.js
│   │      ├── bundle-api.js
│   │      ├── bundle.css
│   │      └── analytics.js
│   │
│   ├── snippets/
│   │      ├── skeleton-loader.liquid
│   │      └── toast.liquid
│   │
│   └── templates/
│          └── product.json
│
└── README.md
```

---

# ⚙️ Installation & Setup

## 1. Backend Setup

```bash
cd server

npm install

cp .env.example .env
```

Configure `.env`:

```env
PORT=3000

SHOPIFY_STORE_DOMAIN=your-store.myshopify.com

SHOPIFY_STOREFRONT_ACCESS_TOKEN=your_token

GEMINI_API_KEY=your_gemini_key

SHOPIFY_API_VERSION=2024-10
```

Run:

```bash
npm run dev
```

---

## 2. Create Public Backend URL

Shopify cannot access localhost directly.

Start ngrok:

```bash
npm run tunnel
```

Example:

```
https://example.ngrok-free.app
```

Use this URL inside Shopify Theme Settings.

---

# 🎨 Shopify Theme Setup

Start theme:

```bash
cd shopify-theme

shopify theme dev --store your-store.myshopify.com
```

In Shopify Theme Editor:

Enable:

✅ AI Recommended Bundle Section

Configure:

* Backend URL
* Section title
* Recommendation count
* Fallback collection

---

# 🧠 AI Recommendation Workflow

```
Product Page Loads

        ↓

Frontend collects product data

        ↓

POST /api/ai/recommendations

        ↓

Gemini generates suggestions

        ↓

Shopify catalog matching

        ↓

Available products returned

        ↓

Bundle cards rendered

        ↓

AJAX Cart API adds products

```

---

# 🚀 Performance & Reliability

Implemented:

✅ Backend caching
✅ Client session caching
✅ API timeout handling
✅ Rate limiting
✅ Loading skeletons
✅ Error states
✅ No jQuery dependency
✅ Deferred JavaScript loading

---

# 🛍️ Cart Integration

Uses Shopify AJAX Cart API:

```
/cart/add.js
```

Supports:

* Single product add
* Complete bundle add
* Multiple line items in one request

---

# 📊 Analytics

Includes basic tracking:

* Recommendation impressions
* Product clicks
* Add-to-cart events

Events are logged through:

```
window.dataLayer
```

---

# 🎥 Demo Flow

The demo shows:

### 1. Product Page

Customer opens a product:

Example:

```
Whey Protein
```

↓

### 2. AI Bundle Generation

AI suggests:

```
Creatine
Mass Gainer
Pre Workout
Multivitamin
```

↓

### 3. Add Bundle

All products are added together using AJAX Cart API.

---

# 📸 Screenshots

## AI Recommendation Section

![AI Bundle](IMAGE_URL)

## Product Matching

![Matching](IMAGE_URL)

## Bundle Cart

![Cart](IMAGE_URL)

---

# 🔮 Future Improvements

Production version can include:

* Shopify App Proxy integration
* Vector embeddings for smarter matching
* Customer purchase history analysis
* Advanced analytics dashboard
* Personalized recommendations

---

# 👨‍💻 Tech Stack

| Technology             | Purpose               |
| ---------------------- | --------------------- |
| Shopify Liquid         | Storefront UI         |
| JavaScript ES6+        | Frontend logic        |
| Node.js                | Backend               |
| Express                | API server            |
| Google Gemini AI       | Recommendation engine |
| Shopify Storefront API | Product data          |
| AJAX Cart API          | Bundle checkout       |

---

## ⭐ Project Goal

Increase Shopify store conversions by automatically creating intelligent product bundles and improving average order value through AI-powered recommendations.


<img width="1341" height="587" alt="image" src="https://github.com/user-attachments/assets/2a8a366d-e95e-422a-929f-6e3e4c7ac939" />

<img width="1000" height="444" alt="Screenshot 2026-07-07 113858" src="https://github.com/user-attachments/assets/f7efb71b-d6f1-4bb2-a540-a88f3d3c381a" />
<img width="1365" height="482" alt="Screenshot 2026-07-07 113805" src="https://github.com/user-attachments/assets/97cc052d-c5ad-4ed1-b52f-01a70d86126c" />
<img width="1365" height="500" alt="Screenshot 2026-07-07 113751" src="https://github.com/user-attachments/assets/a129f78f-8dde-4e70-ac63-81b4869d8c4f" />
