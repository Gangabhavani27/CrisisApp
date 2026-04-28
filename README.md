# CrisisShield AI 🛡️

> **Emergency response & crisis coordination for hospitality** — hotels, resorts, and event venues.

Real-time, AI-assisted command center connecting guests, staff, and responders.

## ✨ Features

**Guest App (`/sos`)** — One-tap SOS, GPS auto-detect, 5 emergency types, voice/text input, ETA confirmation, AI guidance chatbot, multilingual UI (EN/ES/FR/DE).

**Staff Dashboard (`/dashboard`)** — Live KPIs, Leaflet map with severity-colored pulse markers, sortable incident feed, responder assignment, status tracking, internal staff chat, charts (type/severity/avg response time), realtime toast alerts, panic-cluster banner.

**AI Layer** — Severity inference from keywords, panic-cluster detection (≥2 incidents within 60m & 60s), 0-100 priority score (severity + cluster + age), response-time estimator, rule-based guidance NLP for fire/medical/security/evacuation/earthquake/flooding.

**Auth & Roles** — Email+password (localStorage simulated), Guest/Staff/Admin roles, protected routes.

## 🧱 Tech
.DataBase
.NodeJS
.JavaScript
.React  
·Vite 
· TypeScript 
· Tailwind 
· shadcn/ui 
· Zustand (persisted) 
· Leaflet · Recharts 
· sonner 
· react-router v6.

> The original spec called for a Python/FastAPI backend. Since this runs in-browser, the backend is **simulated** via a Zustand store with seed data + a 15s priority recompute loop. Store actions map 1:1 to future REST/WebSocket endpoints (see API Contract below).

## 🚀 Setup

```bash
npm install
npm run dev          # http://localhost:8080
npm run build
```

### Demo accounts (password `demo1234`)

| Role  | Email             |
|-------|-------------------|
| Guest | guest@demo.com    |
| Staff | staff@demo.com    |
| Admin | admin@demo.com    |

Or visit `/auth?demo=guest|staff|admin` to auto-login.

## 📁 Structure

src/
├── App.tsx                 Routes + RoleGate
├── components/
│   ├── BrandLogo, TopBar
│   ├── IncidentsMap.tsx    Leaflet map
│   ├── AnalyticsCharts.tsx Recharts
│   └── StaffChat.tsx
├── pages/
│   ├── Landing, Auth
│   ├── GuestApp.tsx        SOS + Chatbot
│   └── Dashboard.tsx       Live ops
└── lib/
    ├── types, constants
    ├── auth-store.ts       Zustand persisted
    ├── incident-store.ts   Seed data + actions
    ├── ai.ts               Priority/cluster/severity/ETA
    ├── chatbot.ts          Rule-based guidance
    └── i18n.ts             4 languages

ProtoType Link 

https://gilded-liger-a7281e.netlify.app/

## 🔁 Functional Flow

```
Guest taps SOS → GPS captured → Severity inferred → Panic cluster check
   → Incident persisted + multi-channel alerts logged
   → Dashboard updates (toast for high/critical)
   → AI priority score computed → Staff assigns responders
   → Status: pending → in_progress → resolved → analytics update
```

## 📡 API Contract (future backend)

| Store action          | REST                                   | WebSocket event       |
|-----------------------|----------------------------------------|-----------------------|
| `createIncident`      | `POST /incidents`                      | `incident:new`        |
| `updateStatus`        | `PATCH /incidents/:id/status`          | `incident:updated`    |
| `assignResponder`     | `POST /incidents/:id/assign`           | `incident:updated`    |
| `postChat`            | `POST /chat`                           | `chat:message`        |
| `signup` / `login`    | `POST /auth/{signup,login}`            | —                     |

## 🔮 Future Scope

IoT (smoke/panic sensors, wearables)
· real 911/112 dispatch integration
· ML severity model trained on history
· CV crowd-panic detection from CCTV
· native mobile apps with push
· voice-first staff radio transcribed into chat.
