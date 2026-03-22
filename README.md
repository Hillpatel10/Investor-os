# Investor OS — Decision Intelligence Platform

A behaviour-based investing platform that helps users make better financial decisions.

## Features

- 🧠 **Behaviour Engine** — Detects panic selling, FOMO buying, overtrading
- 📊 **Portfolio Tracker** — Real-time prices via Alpha Vantage
- 🎯 **Goal-Based Investing** — Track progress toward financial goals
- 💰 **Tax Optimization** — STCG/LTCG calculator with suggestions
- 🛡️ **Emergency Fund Monitor** — 6-month expense tracker
- 📰 **News + Impact** — Business news with "Impact on You" analysis
- 🔍 **Company Explorer** — Fundamentals & AI-style ratings
- 📚 **Learning Centre** — Financial education cards

## Tech Stack

- **Next.js 14** (App Router)
- **TypeScript**
- **Tailwind CSS**
- **Firebase** (Auth + Firestore)
- **Alpha Vantage API** (stock data)
- **NewsAPI** (business news)
- **Recharts** (charts)
- **shadcn/ui** components

## Setup

### 1. Install dependencies

```bash
npm install
```

### 2. Configure environment variables

Edit `.env.local` and add your Firebase config:

```env
NEXT_PUBLIC_ALPHA_VANTAGE_KEY=Z7DX0WL4EX70CPHO
NEXT_PUBLIC_NEWS_API_KEY=5c3a7cd374f84ea8b9c7331f843de39e

NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
```

### 3. Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create a new project
3. Enable **Email/Password Authentication**
4. Create a **Firestore Database** (start in test mode)
5. Create a web app and copy the config into `.env.local`
6. Add these Firestore composite indexes (or let them auto-create on first query):
   - Collection: `portfolio` — Fields: `uid ASC`
   - Collection: `behaviourLogs` — Fields: `uid ASC`, `date DESC`
   - Collection: `trades` — Fields: `uid ASC`, `date DESC`
   - Collection: `goals` — Fields: `uid ASC`

### 4. Run the development server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

### 5. Demo Mode

The app ships with mock data so you can explore all features without Firebase.
- Demo login: `demo@investoros.in` / `demo1234` (requires Firebase Auth setup)
- All portfolio, behaviour, and news data has realistic fallbacks

## Project Structure

```
investor-os/
├── app/
│   ├── (app)/                    # Protected app routes (auth guard)
│   │   ├── layout.tsx            # Auth guard + bottom nav
│   │   ├── dashboard/page.tsx    # Home tab
│   │   ├── portfolio/page.tsx    # Portfolio tab
│   │   ├── insights/page.tsx     # Behaviour engine tab
│   │   ├── explore/page.tsx      # Company explorer tab
│   │   └── news/page.tsx         # News tab
│   ├── auth/
│   │   ├── layout.tsx
│   │   ├── login/page.tsx
│   │   └── signup/page.tsx
│   ├── layout.tsx                # Root layout
│   ├── page.tsx                  # Root redirect
│   └── globals.css
├── components/
│   ├── ui/                       # Base UI components
│   └── layout/                   # Layout components
│       ├── Header.tsx
│       └── BottomNav.tsx
├── components/features/          # Feature-specific components
│   ├── BehaviourScoreRing.tsx
│   └── AllocationChart.tsx
├── lib/
│   ├── firebase.ts               # Firebase init
│   ├── auth-context.tsx          # Auth provider
│   ├── types.ts                  # TypeScript types
│   └── utils.ts                  # Helpers (tax, formatting, impact)
├── services/
│   ├── api.ts                    # Alpha Vantage + NewsAPI
│   └── firestore.ts              # Firestore CRUD + behaviour logic
├── data/
│   └── mock.ts                   # Realistic mock data (fallback)
└── .env.local                    # Environment variables
```

## Deployment on Vercel

```bash
npm run build
vercel deploy
```

Add all `.env.local` variables to your Vercel project's Environment Variables.

## Firestore Collections

| Collection | Fields |
|---|---|
| `users` | name, email, monthlyExpenses, emergencyFund, riskType |
| `portfolio` | uid, symbol, name, quantity, avgBuyPrice, buyDate, sector |
| `goals` | uid, name, targetAmount, currentAmount, deadline, category |
| `behaviourLogs` | uid, type, date, detail, pointsImpact |
| `trades` | uid, symbol, type, price, quantity, date |

## API Rate Limits

- **Alpha Vantage Free**: 25 requests/day, 5/minute — responses are cached for 5 minutes
- **NewsAPI Free**: 100 requests/day — uses mock fallback if limit hit

## License

MIT
