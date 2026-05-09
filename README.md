
https://snapcart-iota.vercel.app/

# 🛒 Snapcart — 10-Minute Grocery Delivery App

Snapcart is a full-stack grocery delivery application built with **Next.js 16**, featuring real-time order tracking, live delivery maps, AI-powered chat suggestions, and multi-role dashboards for customers, delivery boys, and admins.

---

## ✨ Features

### 👤 Customer
- Browse and search grocery items by category
- Add to cart and checkout with delivery address (map-based)
- Pay via **Cash on Delivery** or **Stripe online payment**
- Track orders in real-time on a live map
- OTP-based delivery confirmation
- View full order history

### 🚴 Delivery Boy
- View and accept delivery assignments
- Real-time GPS location sharing via Socket.IO
- Send/receive OTP to confirm delivery
- In-app chat with AI suggestions

### 🛠️ Admin
- Add, edit, and delete grocery items (with Cloudinary image upload)
- View and manage all orders
- Update order status (`pending` → `out of delivery` → `delivered`)
- Assign delivery boys to orders
- Role management for users

---


## 📁 Project Structure

```
src/
├── app/
│   ├── api/
│   │   ├── auth/           # NextAuth + register
│   │   ├── admin/          # Grocery & order management
│   │   ├── user/           # Orders, cart, payment, Stripe webhook
│   │   ├── delivery/       # Assignments, OTP, location
│   │   ├── chat/           # AI suggestions & message history
│   │   ├── socket/         # Socket.IO connect & location update
│   │   └── me/             # Current user endpoint
│   ├── admin/              # Admin pages (add/view grocery, manage orders)
│   ├── user/               # Customer pages (cart, checkout, orders, tracking)
│   ├── login/
│   ├── register/
│   └── unauthorized/
├── components/             # Shared UI components
├── hooks/                  # Custom React hooks
├── models/                 # Mongoose schemas
│   ├── user.model.ts
│   ├── order.model.ts
│   ├── grocery.model.ts
│   ├── message.model.ts
│   └── deliveryAssignment.model.ts
├── redux/                  # Redux store & slices
├── lib/                    # DB connection utility
├── auth.ts                 # NextAuth config
├── Provider.tsx            # SessionProvider wrapper
└── InitUser.tsx            # Session-aware user initializer
```


## 🧱 Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 16 (App Router) |
| Language | TypeScript |
| Auth | NextAuth v5 (Credentials + Google OAuth) |
| Database | MongoDB with Mongoose |
| State Management | Redux Toolkit |
| Payments | Stripe |
| Image Storage | Cloudinary |
| Real-time | Socket.IO |
| Maps | Leaflet + React Leaflet |
| Styling | Tailwind CSS v4 |
| Animations | Motion (Framer Motion) |
| Email | Nodemailer |

---

## 🔑 Authentication

Snapcart uses **NextAuth v5** with two providers:

- **Credentials** — email + bcrypt-hashed password
- **Google OAuth** — auto-creates a user on first sign-in

Sessions use the **JWT strategy** and are stored as HTTP-only cookies. The session is available both client-side (`useSession`) and server-side (`auth()`).

---

## 💳 Payments

Snapcart integrates **Stripe** for online payments:

1. On checkout, a Stripe payment session is created via `/api/user/payment`
2. User is redirected to Stripe's hosted checkout page
3. On success, Stripe sends a webhook to `/api/user/stripe/webhook`
4. The webhook marks the order as `isPaid: true`

> Make sure to configure your Stripe webhook endpoint in the Stripe dashboard pointing to `YOUR_DOMAIN/api/user/stripe/webhook`.

---

## 🗺️ Real-Time Tracking

Order tracking uses **Socket.IO** and **Leaflet**:

- Delivery boy's location is emitted via socket every few seconds from `GeoUpdater`
- Customer's `track-order` page receives live coordinates and updates the map marker
- The `LiveMap` component renders the delivery route in real time

---

## 🤖 AI Chat Suggestions

The delivery chat screen includes AI-powered reply suggestions fetched from `/api/chat/ai-suggestions`, which calls the Anthropic Claude API to generate context-aware message options based on the current conversation history.
