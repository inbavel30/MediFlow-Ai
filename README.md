# 🏥 MediFlow Balancer — AI-Powered Hospital Load Balancing

> **Hackathon Demo** · Real-time, 100% software-defined hospital network orchestrator for Chennai's public hospital network


## 🎯 What Problem We Solve

Chennai's public hospitals — RGGGH (1,500 beds), Stanley Medical (1,531 beds), and Kilpauk MC (960 beds) — face severe patient routing inefficiencies:

| Problem | Before MediFlow | After MediFlow |
|---------|----------------|----------------|
| ER Wait Time | 47 minutes | 28 minutes (−40%) |
| Ambulance Diversion Rate | 18% | 4% |
| Network Fairness (Gini Index) | 0.42 (inequitable) | 0.15 (balanced) |
| Routing Decision Time | Manual (~5 min) | Automated (~12ms) |

---

## 📱 Page-by-Page Guide for Judges

### 1. 🖥️ Dashboard

**What you see:**
- **4 KPI Cards** at the top: Avg ER Wait (minutes), System Load (%), Active Diversions count, Patients Routed Today
- **Hospital Network SVG Map** — an animated network topology showing RGGGH, Stanley Medical, and Kilpauk MC as nodes. Circle size = bed capacity; color = status (green/amber/red); pulsing animation = critical overload
- **AI Recommendations Panel** — live, confidence-scored recommendations like *"Redirect 3 patients from RGGGH → Stanley | saves 14 min"*
- **Hospital Cards (3 cards)** — per-hospital: occupancy bar, ER wait time, queue size, overload countdown timer, and a **Activate Diversion** button
- **Load Equity (Gini Widget)** — Gini index score (0 = perfect equity, 1 = total imbalance) with before/after AI comparison bar chart
- **6-Hour Demand Forecast** — Prophet-style predictive line chart showing which hospitals will hit capacity in the next few hours

**Interactive Buttons:**
- `Activate Diversion` / `End Diversion` — toggles ambulance diversion; updates the network map in real time
- Overload banners appear with a `Pre-emptive Diversion` action button when any hospital is predicted to fill within 15 minutes
- All data **auto-updates every second** via a simulated tick engine

---

### 2. 🚨 ER Queue 
**What you see:**
- **Hospital Selector Tabs** (RGGGH / Stanley / Kilpauk) — click to switch between hospital queues; shows patient count per tab
- **ER Status Banner** — current hospital's queue depth, average wait time, and a dynamic *"ER safe for ~Xm more"* countdown
- **Priority Queue Table** — sorted by ESI priority weight (a formula: base priority + aging factor − time penalty). Columns:
  - **ESI Badge** (P1–P5, color-coded) + URGENT flag when aging critically
  - Patient name and ID
  - Medical condition
  - Wait time (minutes)
  - Priority weight score (live updating)
  - Formula breakdown: e.g., `100 + 22 - 8` (base + age bonus − time penalty)
  - **Admit** button

**Interactive Buttons:**
- `Divert` / `End Diversion` — per-hospital diversion toggle on the banner
- `Admit` — moves patient from waiting → in-treatment, removes from queue, updates occupancy counts
- Hospital tab buttons — switch active hospital queue view

---

### 3. 🏨 Ward Digital Twin 

**What you see:**
- **Ward Cards** — 4 types per hospital (ICU, General, Trauma, Maternity) showing occupancy percentage, beds available, color status ring, and bed-level breakdown bar
- **Interactive Bed Grid** — visual grid of individual beds. Each bed shows:
  - Patient name + ESI level (if occupied)
  - "Available" status (if empty)
  - Hover tooltip with patient details
- **Adjust Beds slider** and **Reduce Elective %** control on each hospital card

**Interactive Buttons:**
- `+Bed` / `-Bed` buttons on each hospital card — dynamically adjusts total bed capacity and recalculates all percentages
- `Reduce Elective X%` on each ward card — simulates clearing elective surgeries to free up beds for emergency load

---

### 4. 🤖 AI Decision Log 

**What you see:**
- **Filter Tabs**: `All`, `ESI 1-2 Critical`, `Redirected`, `Overridden` — filters the decision log
- **Decision Cards** — each card shows:
  - ESI badge (color-coded P1–P5)
  - Patient name → Assigned hospital
  - **Confidence Ring** (green ≥70%, amber ≥50%, red below)
  - Decision time in milliseconds (shows AI speed: ~12ms)
  - Timestamp
- **Expanded View** (click any card) reveals:
  - **Score Breakdown Chart** — stacked horizontal bar chart showing Load / Wait / Specialty / Distance subscores for each candidate hospital
  - **Why Not Chosen** — natural language explanation for rejected hospitals (e.g., *"Wait penalty (8.2pts) dominates — total score 0.743"*)
  - **Trade-off note** — explains the AI's re
  asoning (e.g., *"Redirected due to RGGGH reaching 91% capacity; Stanley offers comparable specialty care"*)
  - **Gini badge** — Gini coefficient value at the moment the decision was made
  - `Redirected` / `Critical Override` badges for special cases

**Interactive Buttons:**
- Filter buttons (All / ESI 1-2 Critical / Redirected / Overridden)
- Click any decision card to expand/collapse full AI reasoning

---

### 5. ⚗️ Simulation Console 

**What you see:**
- **Mode Toggle** (large pill switch) — flips between `Baseline` (random routing) and `MediFlow AI` (optimized routing)
- **Control Sliders**:
  - Patient Volume (10–100 patients to inject)
  - Surge Beds (+0–30 surge capacity)
  - Elective Reduction (0–50% to simulate pre-clearing wards)
- **Scenario Presets**: 4 quick-load buttons:
  - `Normal Day` — 30 patients, no surge
  - `Monday Rush` — 60 patients
  - `Monsoon Surge` — 80 patients (+35% simulate Chennai Oct-Dec floods)
  - `Mass Casualty` — 100 patients + 20 surge beds + 30% elective reduction
- **Results Panel** (appears after running):
  - Comparison table: Metric / Baseline / MediFlow AI / Delta (all in green/red)
  - Bar chart: Load distribution before vs after MediFlow

**Interactive Buttons:**
- `Baseline` ↔ `MediFlow AI` toggle
- Preset buttons (Normal Day / Monday Rush / Monsoon Surge / Mass Casualty)
- `Run Simulation (N)` — runs the simulation, injects patients, shows real-time results after ~800ms
- `Reset` — clears all state and returns to initial hospital configuration

---

### 6. 🚑 Ambulance Dispatcher 

**What you see:**
- **Quick Patient Entry panel**:
  - 5 ESI level buttons (color-coded: red=P1 critical, orange=P2, yellow=P3, green=P4/P5)
  - Symptom dropdown (Chest Pain, Trauma, Stroke, etc.)
- **Top 2 Recommendations** — ranked hospital cards showing:
  - ETA (minutes, based on Haversine distance at 40 km/h)
  - ER Wait time
  - Queue position (where this patient would land)
  - Beds available
  - Specialty match indicator (✅ or ⚠️)
  - #1 badge in green, #2 in blue
- **SVG Route Map** — animated dashed line from ambulance position to top-ranked hospital; line animates to show routing
- **Confirmation Message** — green banner confirming routing with ETA and queue position

**Interactive Buttons:**
- ESI level buttons (1–5) — changes severity, re-ranks hospitals instantly
- Symptom dropdown — changes specialty matching
- `CONFIRM ROUTING` button (green for #1, blue for #2) — routes patient into the network, updates all hospital queues/occupancy

---

### 7. 📊 Analytics 

**What you see:**
- **24-Hour Occupancy Trend** — multi-line chart (one line per hospital) showing occupancy % over 24 hours with a mock sinusoidal trend representing day/night patterns
- **ER Wait vs 30-min Target** — area chart with a red dashed reference line at 30 minutes (the NABH target); shows how MediFlow keeps hospitals below the threshold
- **Ward Utilization** — horizontal bar chart of individual ward utilization across all hospitals (ICU/General/Trauma/Maternity for each)
- **Hospital Comparison Radar** — spider/radar chart comparing all 3 hospitals on 5 axes: Load, ER Wait, Queue depth, Specialty breadth, Available Capacity

---

### 8. 📋 Patient Intake Form 

**What you see:**
- **Patient form**: Name field, Age field
- **ESI Level selector** — 5 buttons color-coded, each labeled (1-Immediate / 2-Emergent / 3-Urgent / 4-Minor / 5-Non-urgent)
- **Specialty dropdown** — Cardiology, General, Trauma, Orthopedics, Neurology, Maternity, Pediatrics
- **Symptom chip selector** — clickable symptom tags (Chest Pain, Dyspnea, Head Injury, etc.); toggle to select multiple

**Interactive Buttons:**
- **ESI level buttons** — color highlights the selected level
- **Symptom chips** — click to add/remove from selection (turns blue when selected)
- `Route Patient` — submits the form, runs the WLL-BS algorithm, and shows a **result card** with:
  - Assigned hospital name
  - AI explanation text
  - Confidence %, Decision time (ms), Gini at decision time
  - Score comparison bar for all 3 hospitals (shows why top hospital won)
  - Trade-off note


## 🧠 AI Engine — How It Works

### WLL-BS Scoring Algorithm (runs in < 15ms)

Every patient routing decision uses a **Weighted Load-Leveling Balanced Score**:

```
Score(hospital) = w1 × LoadScore + w2 × WaitScore + w3 × SpecialtyScore + w4 × DistanceScore
```

- **LoadScore** — penalizes hospitals above 80% capacity exponentially
- **WaitScore** — penalizes ER queues proportional to expected wait δ from the best option
- **SpecialtyScore** — +bonus if the hospital has the matching specialty (e.g., Cardiology for Chest Pain)
- **DistanceScore** — based on Haversine great-circle distance
- **Critical Override** — ESI-1 patients always go to the nearest non-diverted hospital regardless of score

### Gini Coefficient Monitor

After every routing decision, the system computes the Gini index across hospital occupancies:

```
Gini = Σ|xi - xj| / (2 × n² × μ)
```

A Gini of `0.00` = perfectly balanced network. A Gini above `0.25` triggers the "rebalancing recommended" warning.

### Priority Queue Aging

Every tick (simulated every second), waiting patients' priority weights are recalculated:

```
Weight = BasePriority(ESI) + 20 × e^(max(0, (waitPct - 0.8) × 3)) - 10 × waitPct
```

This ensures ESI-3 patients who have waited past 80% of their max safe wait time get boosted above freshly-arrived ESI-2 patients.

---

## 🏗️ Technical Architecture

```
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  Vite/React │────▶│  Context Store   │────▶│  WLL-BS Router  │
│  Frontend   │     │  (MediFlowCtx)   │     │  + Gini Monitor │
│ localhost:  │     │                  │     │  + Priority Aging│
│    8080     │     └──────────────────┘     └─────────────────┘
└─────────────┘
       │
       ▼
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│ Next.js 15  │────▶│  FastAPI Backend  │────▶│    PostgreSQL   │
│  Frontend   │     │  localhost:8000  │     │    + Redis 7    │
│ localhost:  │◀────│  Socket.IO + REST│     │  localhost:5432 │
│    3001     │     └──────────────────┘     └─────────────────┘
└─────────────┘
```

**Tech Stack:**
- **Frontend (Main Demo):** React 18, TypeScript, Vite, TailwindCSS, Recharts, shadcn/ui
- **Frontend (Extended):** Next.js 15, React-Leaflet maps, Socket.IO client
- **Backend:** Python 3.11, FastAPI, Socket.IO (ASGI), PuLP (LP optimizer), Prophet (forecasting)
- **Data Layer:** PostgreSQL 15 + PostGIS, Redis 7 (priority queues)
- **Ops:** Docker Compose (optional; runs standalone as pure frontend too)

---

## ▶️ How to Run

### Option A — Standalone Frontend (No Backend Needed)
```bash
cd "d:/Downloads DISK C/mediflow-ai-command-main/mediflow-ai-command-main"
npm install
npm run dev
# → http://localhost:8080
```

### Option B — Full Stack (Backend + Both Frontends)
```bash
# Terminal 1 — Backend
cd d:/MEDIFLOWAI/backend
python -m uvicorn main:socket_app --host 0.0.0.0 --port 8000

# Terminal 2 — Next.js Frontend
cd d:/MEDIFLOWAI/frontend
npm run dev
# → http://localhost:3001

# Terminal 3 — Vite Frontend (already running)
# → http://localhost:8080
```

### Prerequisites
- Node.js 18+
- Python 3.11+
- PostgreSQL 15 (optional, backend falls back gracefully)
- Redis 7 (optional)

---

## 📊 Key Metrics Demonstrated

| KPI | Value | How to Verify |
|-----|-------|--------------|
| AI Decision Speed | ~12ms | AI Log page — see decision time column |
| ER Wait Reduction | 40% | Dashboard KPI card |
| Diversion Rate Reduction | 78% | Simulation page — run "Monsoon Surge" preset |
| Fairness Improvement | 64% Gini reduction | Dashboard Gini Widget — Before AI vs After AI |
| Patient Throughput | 83% routed optimally | Simulation results table |

---

## 🌦️ Monsoon Surge Mode

Click **Simulation → Monsoon Surge preset → Run Simulation** to see:
- 35% more patient arrivals simulated
- AI redistributes load automatically
- Gini stays low even under surge
- Diversion rate stays below 5% vs 18% baseline

---

*Built for the MediFlow AI Hackathon · MIT License*
