# 🏙️ CityMove

> **Smart, Community-Powered Urban Transit & Road Intelligence**  
> A 100% free and open-source civic navigation platform helping urban commuters navigate roads, report hazards, and track public transit without proprietary locks or fees.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Frontend](https://img.shields.io/badge/Frontend-React_18_+_Vite_+_Tailwind-61dafb.svg)](frontend/)
[![Backend](https://img.shields.io/badge/Backend-Node.js_+_Express_+_SQLite-green.svg)](backend/)
[![Tests](https://img.shields.io/badge/Tests-25_Passing-brightgreen.svg)](#testing)

---

## 📸 Screenshots

```
+---------------------------------------------------------------------------------------+
|  CityMove  [Route Planner]  [Road Issues]  [Public Transit]                  [☀️ / 🌙] |
+------------------------------------+--------------------------------------------------+
|  Origin:      [ Trafalgar Sq   ]   |                                                  |
|  Destination: [ City Rd Hub    ]   |             (A) Origin Pin                       |
|                                    |               \                                  |
|  [Find Routes]                     |                \====== [Alternative 1] (Green)  |
|                                    |                 \====== [Route 1: Fastest] (Blue)|
|  * Alternative 1: 18 min (4.2 km)  |                 ⚠️ Hazard Marker (Pothole)       |
|  * Alternative 2: 24 min (5.1 km)  |                  \                               |
|                                    |                  (B) Destination Pin             |
|  ⚠️ 2 road hazards on this route    |                                                  |
|                                    |                                                  |
|  [Best Time to Leave Bar Chart]    |                                                  |
|  Est. typical peak-hour patterns   |                                                  |
+------------------------------------+--------------------------------------------------+
```
*(UI screenshots placeholder: `/docs/screenshots/planner.png`, `/docs/screenshots/report.png`, `/docs/screenshots/transit.png`)*

---

## ✨ Features

### 1. Traffic and Route Planner (`/planner`)
- **OSRM Multi-Route Routing**: Queries the Open Source Routing Machine (OSRM) to calculate up to **3 alternative routes** with geometry, distance, and duration metrics.
- **Nominatim Autocomplete**: OpenStreetMap search with automatic 1-second request throttling and User-Agent identification.
- **One-Click Pin Placement**: Set origin and destination using GPS ("Use My Location") or by clicking directly on the interactive Leaflet map.
- **"Best Time to Leave" Chart**: Chart.js visualization displaying expected trip duration across all 24 hours based on an hourly congestion multiplier model (highlighting morning 08:00–10:00 and evening 17:00–20:00 rush hours for weekdays and weekends).
- **Hazard Corridor Warnings**: Automatically detects road hazards within 120m of the active route and warns: *"N reported issues on this route"*.
- **Saved Favorite Routes**: Save frequent trips (e.g., *Home → Office*) to `localStorage` with instant loading and deletion.

### 2. Road Issue Reporting (`/report`)
- **Interactive Pin Placement**: Click anywhere on the map to target potholes, broken signals, waterlogging, or other road hazards.
- **Photo Evidence Upload**: Optional image upload (JPEG/PNG/WebP up to 5 MB) validated server-side.
- **Marker Clustering & Filters**: Filter issues by type, status (Open vs. Confirmed Fixed), and sort by date or upvotes.
- **Crowdsourced Upvoting**: One upvote per client browser enforced via client UUID stored in `localStorage` and verified on the server.
- **Collaborative Verification**: Issues transition to "Fixed" upon receiving **3 independent confirmations** from distinct users.
- **No Login Required**: Zero barrier to entry; protected against abuse using Express rate limiting and text sanitization.

### 3. Public Transport Tracker (`/transit`)
- **Sample Transit Network**: Seeded with **2 bus routes** (`BUS-101`, `BUS-204`) and **1 metro line** (`METRO-M1`) with 8–10 stops each and scheduled timetables.
- **Live Departure Countdown**: Stop search displaying the next 5 departures with an active **second-by-second countdown ticker** based on scheduled timetables.
- **Interactive Route Map**: View colored transit lines and stop markers with real-time popup schedules.
- **"I'm on this bus" Telemetry**: Passengers can opt-in to broadcast their location every 15 seconds. Active rider positions render as pulsing bus beacons on the map and automatically expire after **2 minutes of silence**. No location history is ever recorded.

---

## 🛠️ Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 18, Vite 6, React Router 6, Leaflet 1.9, react-leaflet, Chart.js 4, react-chartjs-2, Tailwind CSS 3, Lucide Icons |
| **Backend** | Node.js 20+, Express 4, SQLite (`better-sqlite3` & `node:sqlite`), Multer, Zod, Cors, Helmet, express-rate-limit |
| **Routing** | OSRM HTTP API (`https://router.project-osrm.org` or self-hosted) |
| **Geocoding** | Nominatim (OpenStreetMap) with debounced queue |
| **Testing** | Vitest (Frontend), Jest + Supertest (Backend) |
| **DevOps** | Docker, Docker Compose, Nginx |

---

## 🚀 Quick Start

### Option A: Running with Docker Compose (Recommended)

Make sure Docker is running on your machine:

```bash
# 1. Clone repository
git clone https://github.com/citymove-org/citymove.git
cd citymove

# 2. Build and start containers
docker compose up --build
```

- **Frontend**: [http://localhost:5173](http://localhost:5173)
- **Backend API**: [http://localhost:3001](http://localhost:3001)

To seed sample transit lines and reports in the Docker container:
```bash
docker compose exec backend npm run seed
```

---

### Option B: Running Locally without Docker

#### 1. Install Dependencies
```bash
# In project root:
cd backend && npm install
cd ../frontend && npm install
```

#### 2. Seed the Database
```bash
cd backend
npm run seed
```

#### 3. Start the Backend API
```bash
cd backend
npm run dev
# Server starts on http://localhost:3001
```

#### 4. Start the Frontend
In a new terminal:
```bash
cd frontend
npm run dev
# Vite dev server starts on http://localhost:5173
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## ⚙️ Environment Variables

### Backend (`backend/.env`)
| Variable | Default | Description |
|---|---|---|
| `PORT` | `3001` | Express server port |
| `NODE_ENV` | `development` | Runtime environment |
| `CLIENT_ORIGIN` | `http://localhost:5173` | Allowed CORS frontend origin |
| `OSRM_URL` | `https://router.project-osrm.org` | OSRM routing base URL |
| `DB_PATH` | `./citymove.db` | SQLite database file location |
| `UPLOAD_DIR` | `./uploads` | Local directory for report photos |
| `RATE_LIMIT_WINDOW_MS` | `60000` | Rate limiting window (1 min) |
| `RATE_LIMIT_MAX_REQUESTS`| `200` | Max requests per IP per window |

### Frontend (`frontend/.env`)
| Variable | Default | Description |
|---|---|---|
| `VITE_API_URL` | `http://localhost:3001` | Backend API URL |
| `VITE_OSRM_URL` | `https://router.project-osrm.org` | OSRM routing base URL |

---

## 🧪 Testing

The test suite covers API routes, validation rules, upvote concurrency, spatial proximity calculations, traffic models, and UI components with **25 passing tests**:

```bash
# Run backend Jest tests (17 tests)
cd backend && npm test

# Run frontend Vitest tests (8 tests)
cd frontend && npm test

# Or run both from root
npm test
```

---

## 🗺️ Self-Hosting OSRM with Docker

The default OSRM public server (`https://router.project-osrm.org`) is rate-limited and intended for development/demos only. For production deployments or heavy workloads, run a local OSRM instance:

```bash
# 1. Download OpenStreetMap extract for your region (e.g. Great Britain or Berlin)
wget http://download.geofabrik.de/europe/great-britain/england/greater-london-latest.osm.pbf

# 2. Extract and contract the road graph
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-extract -p /opt/car.lua /data/greater-london-latest.osm.pbf
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-partition /data/greater-london-latest.osrm
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-customize /data/greater-london-latest.osrm

# 3. Spin up the OSRM routing daemon
docker run -t -i -p 5000:5000 -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-routed --algorithm mld /data/greater-london-latest.osrm
```

Then point your `OSRM_URL` environment variable to `http://localhost:5000` in `backend/.env` and `frontend/.env`.

---

## ⚠️ Known Limitations

1. **Estimated Traffic Congestion**: There is no free, global live-traffic API. Travel time delay and peak-hour bar charts are calculated using a realistic statistical multiplier model (`src/config/trafficModel.js`) calibrated for typical weekday (08:00–10:00, 17:00–20:00) and weekend rush patterns, not sensor/probe feeds.
2. **Sample Transit Data**: Bus and metro routes, schedules, and initial rider locations are sample data created for demonstration and testing purposes.
3. **Public API Usage Limits**: The public demo servers for OpenStreetMap tiles, Nominatim geocoding, and OSRM have strict fair-use quotas (Nominatim allows max 1 request/second, which CityMove enforces via an in-memory rate-limiting queue). Self-host OSRM and tile servers for high-volume deployments.

---

## 📖 API Documentation

Complete REST endpoint specifications, schemas, and examples are documented in [`docs/API.md`](docs/API.md).

---

## 🌟 Open-Source Credits

CityMove is built on top of remarkable open-source projects and open data:

- [OpenStreetMap](https://www.openstreetmap.org/): Open map data & cartography (&copy; OpenStreetMap contributors).
- [Project OSRM](http://project-osrm.org/): High-performance routing engine for shortest paths in road networks.
- [Nominatim](https://nominatim.org/): OpenStreetMap search and reverse geocoding engine.
- [Leaflet](https://leafletjs.com/) & [react-leaflet](https://react-leaflet.js.org/): Mobile-friendly interactive map library.
- [Chart.js](https://www.chartjs.org/) & [react-chartjs-2](https://react-chartjs-2.js.org/): Accessible HTML5 Canvas charts.
- [Lucide Icons](https://lucide.dev/): Crisp, accessible UI iconography.
- [Tailwind CSS](https://tailwindcss.com/): Utility-first CSS framework.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

