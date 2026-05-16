# WithHer — Product Requirements Document

> *"You don't have to figure this out alone."*

---

## 1. Problem

Migrant women in Australia face a healthcare system that is fundamentally different from what they grew up with. This confusion doesn't go away with time — it resurfaces every time they face a new health situation.

Two core fears drive this:

**Fear 1: Not knowing where to go**
> "I have a headache. Do I go to the emergency room, book a GP, or just go to the pharmacy?"

Choosing wrong means wasted money, wasted time, or worse — ignoring something serious.

**Fear 2: Not knowing what it will cost**
> "I have insurance, but I have no idea how much I'll actually have to pay out of pocket today."

The fear isn't always about affording it. It's about the uncertainty. Not knowing is its own kind of stress.

---

## 2. Target User

**Migrant women in Australia, aged 18–35**

International students, recent migrants, and women who have lived here for years but still feel lost navigating the system. The confusion isn't about language — it's about a system that works completely differently from home.

This product is built **by a migrant woman, for migrant women**. We don't serve everyone — we serve her.

---

## 3. Product Voice

**Professional but warm** — like a knowledgeable friend who happens to understand the Australian healthcare system. She explains things clearly, never makes you feel stupid for not knowing, and always tells you what to do next.

Not clinical. Not condescending. Not overwhelming.

---

## 4. Core Features (MVP)

| Feature | Route | Priority | Status |
|---------|-------|----------|--------|
| Homepage — clear entry point | `/` | High | 🔄 In progress |
| Triage tool — situation → guidance | `/guide` | High | ⬜ Planned |
| Health guides — plain language articles | `/learn` | High | ⬜ Planned |
| Find care — filtered clinic search | `/care` | High | ⬜ Planned |
| My health hub — saved clinics, checklist | `/hub` | Medium | ⬜ Planned |
| Community & support | `/community` | Medium | ⬜ Planned |

---

## 5. Homepage Structure

The homepage answers three questions in 30 seconds:
1. Is this for me?
2. What does this do?
3. What do I do next?

**Section order:**
1. **Hero** — User recognises herself + one clear action (→ Triage tool)
2. **Trust** — Why believe WithHer? What makes it different?
3. **Feature overview** — What can I do here?
4. **CTA** — Come back whenever you need it

---

## 6. Triage Tool (`/guide`)

The centrepiece feature. Not a search engine, not a symptom checker — a **decision-making tool**.

**User flow:**
1. User lands on `/guide` from homepage CTA
2. Selects her situation from 4–5 options
3. WithHer immediately shows: where to go, what to expect, rough costs
4. AI personalisation: optional follow-up input → Claude API returns tailored advice
5. Related health guides surfaced at the bottom

**Situation options (initial):**
- I feel unwell and don't know where to go
- I need to see someone but I'm worried about the cost
- I have a bill or prescription I don't understand
- I want to get a routine check-up but don't know where to start

**Implementation approach:**
- Static content loads instantly (no waiting)
- Claude API called only if user provides additional detail
- API call proxied through backend (API key never exposed to frontend)

---

## 7. Health Guides (`/learn`)

Plain language articles written like that knowledgeable friend. No jargon. No walls of text.

**Initial categories:**
- Medicare — what it is, how to use it, what's free
- GP vs Emergency vs Pharmacy — when to go where
- Reproductive health in Australia
- Mental health — what's available, what's covered
- Understanding your bill

**Content model:**
- Currently authored manually
- Designed to support future contributor submissions (`status: draft | published`)

---

## 8. Find Care (`/care`)

Helps her find the right clinic — not just the nearest one.

**Filters:**
- Bulk billing (free appointments)
- Languages spoken
- Telehealth available
- Suburb / location

**Key decision:** WithHer does not handle bookings. Clinic listings link directly to HotDoc or HealthEngine. We are a connector, not a platform. This keeps the product focused and avoids the complexity of managing appointments.

---

## 9. Design Principles

- **Warm, not clinical** — this is a stressful topic; the design should reduce anxiety
- **Plain language always** — if a 16-year-old couldn't understand it, rewrite it
- **Mobile-first** — she's probably reading this on her phone
- **One thing at a time** — never overwhelm with choices
- **Built for her** — not gender-neutral, not trying to serve everyone

**Brand palette:**
| Role | Name | Hex |
|------|------|-----|
| Primary | Warm navy | `#1B3A5C` |
| Accent | Blush | `#E8B4B8` |
| Background | Soft white | `#F9F8F7` |
| Text | Deep navy | `#0D1F33` |
| Surface | White | `#FFFFFF` |

---

## 10. Tech Stack

| Layer | Technology | Reason |
|-------|-----------|--------|
| Frontend | Vue 3 + Vite | Component-based, reactive, industry standard |
| Backend | Node.js + Express | Same language as frontend, widely used in AU |
| Database | PostgreSQL (Supabase) | Relational, free tier, visual interface |
| AI | Claude API (Anthropic) | Powers triage personalisation, proxied via backend |
| Auth | Firebase Auth | Already integrated, handles complexity |
| Deployment | Vercel (FE) + Render (BE) + Supabase | All free tier, production-grade |

---

## 11. Database Schema

```sql
-- User profiles (auth handled by Firebase)
users
  id              UUID PRIMARY KEY
  firebase_uid    VARCHAR UNIQUE NOT NULL
  email           VARCHAR
  preferred_lang  VARCHAR
  created_at      TIMESTAMP DEFAULT NOW()

-- Recommended clinics
providers
  id                  UUID PRIMARY KEY
  name                VARCHAR NOT NULL
  address             VARCHAR
  suburb              VARCHAR
  state               VARCHAR DEFAULT 'VIC'
  languages           TEXT[]
  bulk_billing        BOOLEAN DEFAULT FALSE
  telehealth          BOOLEAN DEFAULT FALSE
  hotdoc_url          VARCHAR
  healthengine_url    VARCHAR
  source              VARCHAR DEFAULT 'manual'  -- 'manual' | 'self_registered'
  created_at          TIMESTAMP DEFAULT NOW()

-- Health guide articles
guides
  id              UUID PRIMARY KEY
  title           VARCHAR NOT NULL
  content         TEXT
  category        VARCHAR   -- 'medicare' | 'gp_vs_er' | 'mental_health' | 'reproductive' | 'billing'
  language        VARCHAR DEFAULT 'en'
  author_id       UUID REFERENCES users(id)
  status          VARCHAR DEFAULT 'published'  -- 'draft' | 'published'
  created_at      TIMESTAMP DEFAULT NOW()
  updated_at      TIMESTAMP DEFAULT NOW()

-- Triage situations and their content
triage_situations
  id              UUID PRIMARY KEY
  situation_key   VARCHAR UNIQUE NOT NULL  -- 'unwell' | 'cost_concern' | 'billing' | 'checkup'
  title           VARCHAR NOT NULL
  static_content  TEXT    -- shown immediately, no API needed
  ai_prompt       TEXT    -- system prompt sent to Claude for personalisation
  created_at      TIMESTAMP DEFAULT NOW()
```

---

## 12. API Routes (Planned)

```
GET  /api/providers          — list clinics (with filters)
GET  /api/providers/:id      — single clinic detail

GET  /api/guides             — list published guides (with category filter)
GET  /api/guides/:id         — single guide

GET  /api/triage             — list all triage situations
GET  /api/triage/:key        — single situation by key
POST /api/triage/ask         — send user input to Claude, return AI response

GET  /api/users/me           — current user profile (auth required)
PUT  /api/users/me           — update profile (auth required)
```

---

## 13. Roadmap

### Phase 1 — Foundation ✅
Folder structure, Tailwind, brand palette, global navbar

### Phase 2 — Homepage rework 🔄
New homepage UI, correct brand colours, routing connected

### Phase 3 — Core features ⬜
Triage tool, health guides, find care with filters

### Phase 4 — Backend + AI ⬜
Express API, Supabase schema, Claude API integration for triage

### Phase 5 — Polish ⬜
Mobile responsiveness, loading states, error handling, performance

---

## 14. Out of Scope (for now)

- In-app appointment booking (users redirected to HotDoc / HealthEngine)
- Multi-role user system (Pro Dashboard deprioritised)
- Native mobile app
- Content in languages other than English (designed for, not yet implemented)

---

*Last updated: 16 May 2026*
*Author: Leticia Liao*