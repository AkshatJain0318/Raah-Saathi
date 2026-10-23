# RaahSaathi — Hyper-Local Indore City Bus & Daily Commute Portal

> Centralized real-time transit portal for Indore City Buses (AICTSL i-Bus BRTS, City Midi Routes M-4, R-5, Electric Route E-2, and Ring Road Feeders). Built with React PWA, Node.js, Socket.io, Redis Geospatial, and Leaflet Google Maps styled GIS.

---

## 📁 Project Directory Structure

```text
raahsaathi-indore-city-bus/
├── index.html                   # Standalone single-file interactive PWA dashboard (Ready to run in any browser)
├── README.md                    # Project documentation & setup guide
├── frontend/                    # React 18 Progressive Web App
│   ├── package.json             # React, Vite/CRA, react-leaflet, socket.io-client, tailwindcss
│   ├── public/
│   │   ├── manifest.json        # PWA Web App Manifest
│   │   └── service-worker.js    # Offline timetable & stop queue caching engine
│   └── src/
│       ├── App.jsx              # Main Dashboard Component with tab routing
│       └── components/
│           ├── IndoreGoogleMap.jsx   # Interactive Google Maps-styled Leaflet map
│           ├── RoutePlanner.jsx      # 3 recommendation algorithms (Least Wait, Cheapest, Fewest Transfers)
│           ├── StopCrowdAdvisor.jsx  # Stop queue monitoring & upstream seat-finding advisor
│           ├── CarpoolSharing.jsx    # P2P Cab/Carpool matching for daily commuters
│           └── StreakPerks.jsx       # 100-day streak engine with weekend holiday protection
├── backend/                     # Node.js + Express + Socket.io Server
│   ├── package.json             # Express, Socket.io, ioredis, cors
│   ├── server.js                # WebSocket corridor rooms & consensus broadcast
│   ├── redisGeoEngine.js        # Redis GEOADD, GEORADIUS (150m proximity check)
│   └── .env.example             # Environment variables configuration
└── docs/
    ├── ARCHITECTURE.md          # Complete system & data-flow architecture
    ├── API_SPEC.md              # REST & WebSocket event specifications
    └── INDORE_38_STOPS_DATA.json# Comprehensive dataset of 38+ Indore stops & GPS coordinates
```

---

## 🚀 Quick Setup & Run Instructions

### 1. Prerequisites
- **Node.js**: v18.0.0 or higher
- **Redis Server**: v6.2 or higher (`redis-server`)
- **Package Manager**: npm or yarn

### 2. Backend Setup (Node.js + Socket.io + Redis)
```bash
cd backend
npm install
# Ensure Redis is running locally:
# redis-cli ping -> PONG
node server.js
```
The telemetry server will start on `http://localhost:4000`.

### 3. Frontend Setup (React PWA)
```bash
cd frontend
npm install
npm run dev
```
Open `http://localhost:5173` in your browser.

### 4. Or Run Directly Without Node.js
Simply double-click or serve `index.html` with any local HTTP server:
```bash
npx serve .
# Or Python
python3 -m http.server 8080
```
Open `http://localhost:8080/index.html`.

---

## 🌟 Key Features Implemented

1. **Google Maps-Styled Live Leaflet Map**:
   - Zero-API-key vector GIS layer with classic Google Maps palette (`#f1f3f4` land, `#ceead6` parks, `#aadaff` lakes/rivers).
   - Live traffic jam overlay (Red = Heavy delay at Navlakha/Bhawarkuan, Yellow = Moderate).
   - 38+ plotted bus stations with live queue counts and 8 moving AICTSL buses.
   - Map Type Toggle (`🗺️ Road Map`, `🛰️ Satellite Hybrid`, `🚦 Traffic Layer`).

2. **Smart 3-Way Route Recommendations**:
   - **⚡ Least Waiting Time** (Fastest next departure)
   - **💰 Cheapest Route** (Economical student/commuter fare)
   - **🔄 Fewest Bus Changes** (Direct 0-transfer corridors)

3. **Bus Stop Crowd & Upstream Boarding Advisor**:
   - Advises commuters to walk 350-450m to an upstream stop (e.g., Vishnupuri instead of Bhawarkuan, Satya Sai instead of Vijay Nagar) to get a guaranteed seat before rush-hour boarding.

4. **Crowdsourced Verification & Reputation Engine**:
   - Triangulates reports via Redis `GEORADIUS` (150m corridor proximity).
   - Upgrades reports from Pending to Verified upon 3 independent passenger confirmations.
   - Awards Scout Reputation Points (+15 pts for reports, +5 pts for confirmations).

5. **P2P Daily Commute Cab & Carpool Sharing**:
   - Matches college students (DAVV, Medicaps, IET, IIM) and corporate commuters (TCS, Infosys, Pithampur SEZ).
   - Split fuel fare calculator with seat reservation system.

6. **Weekend-Protected Streak & Rewards Shield**:
   - Protects streaks on Saturdays and Sundays based on user schedule (5-Day Office, 2nd/4th Sat Bank/College, or 6-Day Shift).
   - Unlocks Flat 25% Monthly Pass Discount at 100-day milestone.

7. **Smart City Bus Extras**:
   - **Live BRTS ↔ Feeder Connection Sync** with 1-tap conductor hold ping.
   - **Pink i-Bus & Women Night Safety SOS** with AICTSL control room telemetry.
   - **Instant NCMC / Chalo Turnstile QR Ticket** with automated streak discount.
