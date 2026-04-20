# ☕ Starbucks India — Premium Website Clone

A fully interactive, production-ready Starbucks India website with animated homepage, full menu, cart, auth, and Razorpay payment integration.

---

## 🖥️ Live Preview (Frontend Only — No Backend Needed)

> The frontend works **100% standalone** with localStorage. You only need the backend for real database storage + real Razorpay payments.

Simply open `frontend/index.html` in your browser — **no server required** for the frontend demo.

---

## ✨ Features

| Feature | Details |
|---|---|
| 🎨 **Hero Page** | Animated coffee visuals, floating badges, smooth entrance animations |
| 📜 **Drink Showcase** | Alternating left/right scroll sections — cup slides in from left, info from right (and vice versa) |
| 🎠 **Carousel** | Horizontal scrolling Barista Recommends section with skeleton loading |
| 🍔 **Full Menu** | 30+ items across Bestseller, Drinks, Food, Merchandise categories |
| 🔍 **Live Search** | Filter menu items in real-time by name or description |
| 🛒 **Cart** | Add/remove/update quantities, auto price calculation with GST |
| 💳 **Razorpay** | Full payment integration with order creation and webhook verification |
| 🔐 **Auth** | Register/Login with JWT — guests can browse, login required to checkout |
| ✍️ **Typing Facts** | Auto-cycling ChatGPT-style typing animation with Starbucks facts |
| 📱 **Responsive** | Fully mobile-optimized |
| 🔔 **Toast Notifications** | Smooth animated toast for all actions |
| 💀 **Skeleton Loading** | Skeleton screens for carousel and menu items |
| 🎯 **Promo Codes** | Try: `SBUX10`, `FIRST20`, `INDIA15` |

---

## 📁 Project Structure

```
starbucks/
├── frontend/
│   ├── index.html          ← Homepage with animations
│   ├── menu.html           ← Full menu with filters
│   ├── cart.html           ← Cart + checkout + Razorpay
│   ├── login.html          ← Auth (Login + Register)
│   ├── css/
│   │   ├── main.css        ← Global styles, navbar, hero, animations
│   │   ├── menu.css        ← Menu page styles
│   │   ├── cart.css        ← Cart page styles
│   │   └── auth.css        ← Auth page styles
│   └── js/
│       ├── app.js          ← Shared: cart store, toast, ripple, typing
│       ├── menu.js         ← Menu data, filtering, search, modal
│       ├── cart.js         ← Cart render, Razorpay payment flow
│       └── auth.js         ← Login/Register with demo fallback
│
├── backend/
│   ├── server.js           ← Express app entry point
│   ├── package.json
│   ├── .env.example        ← Copy to .env and fill values
│   ├── routes/
│   │   ├── auth.js         ← POST /api/auth/register, /login, GET /me
│   │   ├── menu.js         ← GET /api/menu, /api/menu/:id
│   │   ├── orders.js       ← POST/GET /api/orders
│   │   └── payments.js     ← Razorpay create-order, verify, webhook
│   └── models/
│       └── db.js           ← MySQL2 connection pool
│
└── database/
    └── schema.sql          ← Full MySQL schema + seed data
```

---

## 🚀 Quick Start — Frontend Only (Instant)

No installation needed. Works with any static file server or directly in browser.

```bash
# Option 1: Just double-click frontend/index.html in your file browser

# Option 2: Use Python's built-in server
cd starbucks/frontend
python3 -m http.server 3000

or 

python -m http.server 5500
# Open http://localhost:3000

# Option 3: Use Node's http-server
npx http-server frontend -p 3000
# Open http://localhost:3000
```

**Demo mode:** Login/Register works without backend (stores in localStorage). Cart and Razorpay run in test/demo mode.

---

## 🔧 Full Stack Setup (With Backend)

### Prerequisites
- **Node.js** v18+ — [nodejs.org](https://nodejs.org)
- **MySQL** 8.0+ — [mysql.com](https://www.mysql.com)
- **Razorpay Account** (free test account) — [razorpay.com](https://razorpay.com)

---

### Step 1 — Database Setup

```sql
-- In MySQL:
mysql -u root -p < starbucks/database/schema.sql
```

This creates the `starbucks_db` database with all tables and seed data.

---

### Step 2 — Backend Setup

```bash
cd starbucks/backend

# Install dependencies
npm install

# Create .env file
cp .env.example .env
# Edit .env with your values (DB password, Razorpay keys, etc.)
```

**Edit `.env`:**
```
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_mysql_password
DB_NAME=starbucks_db
JWT_SECRET=any_long_random_string
RAZORPAY_KEY_ID=rzp_test_xxxxxxxxxxxx
RAZORPAY_KEY_SECRET=xxxxxxxxxxxxxxxxxxxxxx
```

```bash
# Start backend server
npm run dev          # Development (auto-reload)
# OR
npm start            # Production
```

Backend runs at: **http://localhost:5000**

---

### Step 3 — Connect Frontend to Backend

In `frontend/js/app.js`, line 7:
```js
const API_BASE = 'http://localhost:5000/api';  // Already set
```

In `frontend/js/cart.js`, line 93:
```js
key: 'rzp_test_YOUR_KEY_HERE',  // Replace with your Razorpay test key
```

In `frontend/cart.html`, also add your Razorpay key to the checkout options.

---

### Step 4 — Open Frontend

```bash
cd starbucks/frontend
python3 -m http.server 3000
```

Open **http://localhost:3000**

---

## 💳 Razorpay Integration

### Getting Test Keys (Free)

1. Go to [razorpay.com](https://razorpay.com) → Sign Up (free)
2. Dashboard → Settings → API Keys
3. Generate Test Key — copy `Key ID` and `Key Secret`
4. Add to `.env`:
   ```
   RAZORPAY_KEY_ID=rzp_test_xxxxxxxxxx
   RAZORPAY_KEY_SECRET=xxxxxxxxxxxxxxxxxx
   ```
5. Add `RAZORPAY_KEY_ID` to `frontend/js/cart.js` line ~93

### Test Payment Cards
| Card Number | Expiry | CVV |
|---|---|---|
| 4111 1111 1111 1111 | Any future date | Any 3 digits |
| 5267 3181 8797 5449 | Any future date | Any 3 digits |

**Test UPI:** Use `success@razorpay` for successful payment

### APIs Used
- `POST /v1/orders` — Create Razorpay order
- `GET /v1/payments/:id` — Fetch payment details
- `GET /v1/payments/downtimes` — Check payment method downtimes
- `POST` Webhook — Verify and update order on payment capture

---

## 🎨 Animations Explained

| Animation | How It Works |
|---|---|
| **Drink Showcase** | IntersectionObserver triggers CSS class that animates translateX from ±60px → 0. Alternates left/right per drink section. |
| **Typing Facts** | Pure JS — cycles through FACTS array, types each character with random 40-60ms delay, then erases. Changes every fact. |
| **Hero entrance** | CSS animation with staggered `animation-delay` for each element. |
| **Carousel skeleton** | Fake cards shown for 800ms, then replaced with real data. |
| **Toast notifications** | Slide in from right, auto-dismiss after 3s, fade out. |
| **Ripple effect** | Click event creates absolutely-positioned div, animates scale 0→4. |
| **Page loader** | SVG circle stroke-dashoffset animation, hidden after 600ms. |

---

## 🧪 Promo Codes (Test)

| Code | Discount |
|---|---|
| `SBUX10` | 10% off |
| `FIRST20` | 20% off |
| `INDIA15` | 15% off |

---

## 🌐 API Endpoints

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/auth/register` | No | Create account |
| POST | `/api/auth/login` | No | Login |
| GET | `/api/auth/me` | JWT | Get current user |
| GET | `/api/menu` | No | Get all products |
| GET | `/api/menu?cat=drinks` | No | Filter by category |
| GET | `/api/menu/:id` | No | Get single product |
| POST | `/api/orders` | JWT | Create order |
| GET | `/api/orders/history` | JWT | User's order history |
| GET | `/api/orders/track/:num` | No | Track by order number |
| POST | `/api/payments/create-order` | JWT | Create Razorpay order |
| POST | `/api/payments/verify` | JWT | Verify payment signature |
| GET | `/api/payments/downtimes` | No | Check payment downtimes |
| POST | `/api/payments/webhook` | Razorpay | Webhook handler |

---

## 🔒 Security Features

- Passwords hashed with **bcrypt** (12 rounds)
- JWT authentication with 7-day expiry
- **Razorpay webhook signature verification** using HMAC-SHA256
- Helmet.js security headers
- CORS restricted to frontend origin
- SQL injection protection via parameterized queries

---

## 📱 Mobile Support

- Sticky navbar with hamburger menu
- Responsive grid (3-col → 2-col → 1-col)
- Touch-friendly buttons and controls
- Floating cart button for mobile
- Scroll-based animations optimized for mobile

---

## 🛠️ Troubleshooting

**CORS error in browser?**
→ Make sure backend is running on port 5000 and `FRONTEND_URL` in `.env` matches your frontend URL.

**MySQL connection refused?**
→ Check MySQL is running: `sudo service mysql start` (Linux) or start MySQL from System Preferences (Mac).

**Razorpay not loading?**
→ Make sure you replaced `rzp_test_YOUR_KEY_HERE` with your actual test key ID in `cart.js`.

**Fonts not loading?**
→ You need an internet connection for Google Fonts. For offline use, download and host fonts locally.

---

## 📊 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3 (Custom Properties), Vanilla JavaScript ES6+ |
| Animations | Pure CSS transitions/animations + IntersectionObserver API |
| Backend | Node.js 18+ + Express 4 |
| Database | MySQL 8 + mysql2 driver |
| Auth | JWT (jsonwebtoken) + bcryptjs |
| Payments | Razorpay Node.js SDK |
| Security | Helmet.js + CORS |

---

## ©️ Disclaimer

This project is built for educational and portfolio purposes. Starbucks® branding, logos, and trademarks belong to Starbucks Corporation. This is not affiliated with or endorsed by Starbucks.
