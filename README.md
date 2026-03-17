# GoDine（搞定）🍽️

別隨便，搞定今天吃什麼 — AI-powered dining recommendations for Taiwan, personalized to your taste.

> Context-aware restaurant suggestions in under 3 seconds. Not a random spinner — a smart recommendation engine that learns from you.

---

## What It Does

GoDine eliminates the daily "今天吃什麼" decision fatigue. Describe what you're in the mood for, and it combines your preferences, location, weather, time, and past choices to instantly suggest 2–3 restaurants with clear reasons.

**Context-aware** — Knows the time, weather, your location, and who you're with. A rainy Tuesday lunch alone gets different suggestions than a sunny Saturday dinner with friends.

**Learns from you** — Every choice you make feeds back into your preference profile. GoDine gets better the more you use it.

**Explainable** — Every recommendation comes with a one-line reason so you know *why* it was suggested, not just *what*.

**Cross-product power** — Phase 2 integrates RTA (are the reviews trustworthy?) and CardSense (which credit card gets you the best deal?).

---

## How It Works

```
User: "兩個人晚餐，想吃日式，預算 $800 以內"

→ Context: 19:00, sunny, Xinyi District
→ Profile: likes spicy, no sashimi, picked ramen last time
→ LLM + Google Maps: generate 3 diverse options with reasons
→ (Phase 2) RTA: verify review trustworthiness per restaurant
→ (Phase 2) CardSense: recommend best payment card
→ Display 3 cards → User picks one → Feedback loop
```

---

## Architecture

```
┌────────────────────────────────────────────┐
│  PWA Frontend (Next.js)                    │
│  Quick input → Results display → Feedback  │
└──────────────┬─────────────────────────────┘
               │
┌──────────────▼─────────────────────────────┐
│  Recommend Engine        Context Collector  │
│  LLM + preference match  Weather / Location │
├─────────────────────────────────────────────┤
│  User Profile Service    Integration Layer  │
│  Preferences + history   RTA · CardSense ·  │
│  Learning model          Google Maps        │
└─────────────────────────────────────────────┘
```

---

## Tech Stack

| Layer | Choice |
|-------|--------|
| Frontend | Next.js (App Router) · PWA manifest · shadcn/ui · Tailwind |
| API | Next.js API Routes |
| Database | PostgreSQL (Supabase) |
| LLM | Claude Sonnet (recommendation quality) · Gemini Flash (intent classification) |
| External APIs | Google Maps Places API · OpenWeatherMap |
| Deploy | Vercel |

---

## Data Model

6 core tables: `users`, `preferences`, `recommendations`, `scenario_templates`, `group_sessions` (Phase 3), `place_cache`.

Recommendation engine logic: collect context → load user profile + history → LLM generates 3 options → (Phase 2) RTA + CardSense enrichment → rank by composite score → return top 3.

---

## Business Model

| Tier | Price | Features |
|------|-------|----------|
| Free | $0 | 5 recommendations/day, basic preferences |
| Pro | $3.99/mo | Unlimited, weather/location context, choice analytics, group decisions |

Phase 3: Affiliate marketing — partner restaurants embedded in recommendations with clear disclosure.

---

## Roadmap

```
Phase 0 ✅  Concept defined, cross-product integration points confirmed
Phase 1     MVP PWA — "今天吃什麼" single scenario, 100 beta users
Phase 2     CardSense + RTA integration, 500 weekly active users, $200 MRR
Phase 3     Affiliate marketing + group decisions, $500 MRR
```

---

## Cross-Product Integration

Part of the [Waddle Studio](https://github.com/WaddleStudio) product portfolio.

| Integration | Description | Phase |
|-------------|-------------|-------|
| → RTA | Verify review trustworthiness for recommended restaurants | Phase 2 |
| → CardSense | Recommend best payment card for the chosen restaurant | Phase 2 |
| ← FridgeManager | Expiring ingredients trigger "what to cook" recommendations | Phase 2 |

Full spec: [`fleet-command/SmartChoice-Spec.md`](https://github.com/WaddleStudio/fleet-command)

---

## Development

```bash
# Prerequisites
Node.js 20+

# Setup
npm install
cp .env.example .env  # fill in Supabase, Google Maps, OpenWeatherMap, LLM keys
npm run dev
```

---

## Legal

- Google Maps Platform API ToS compliance required
- Affiliate recommendations clearly labeled (台灣公平交易法)
- Location data used only per-request, no persistent tracking
- User data deletable on request

---

## License

Proprietary. All rights reserved.

---

*Maintained by [Waddle Studio](https://github.com/WaddleStudio) | Spec: [SmartChoice-Spec.md](https://github.com/WaddleStudio/fleet-command)*
