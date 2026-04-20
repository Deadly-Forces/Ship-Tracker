# 🚢 Sea Tracker — Real-Time Global Maritime Intelligence

A production-grade, real-time global sea traffic tracking system. Like FlightRadar24, but for everything on the ocean.

## Features

- **Live AIS Tracking** — Real-time vessel positions from AISStream, Kystverket, and NOAA
- **Interactive Map** — Dark nautical map with vessel markers, clustering, and trails
- **12+ Map Layers** — OpenSeaMap, bathymetry, wind, waves, submarine cables, EEZ, ports
- **Vessel Detail Panels** — Identity, motion, position, voyage, dimensions
- **Analytics Dashboard** — Vessel type/flag/source breakdowns, speed leaderboard
- **Incident Detection** — Automated MAYDAY, MOB, aground, collision risk detection
- **Alert System** — Speed anomalies, zone entry/exit, military detection
- **History Playback** — Track replay with CSV/GPX export
- **Port Database** — World ports from OpenStreetMap
- **Collision Detection** — CPA/TCPA proximity calculations
- **Zone Monitoring** — Custom geofencing with entry/exit alerts

## Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend | React 18, Vite, Leaflet, Recharts, Zustand, Tailwind CSS |
| Backend | Python 3.11, FastAPI, Uvicorn |
| Database | PostgreSQL + PostGIS |
| Cache/PubSub | Redis |
| Background Tasks | Celery (solo pool for Windows) |
| AIS Data | AISStream.io, Kystverket, NOAA, Global Fishing Watch |
| Weather | Open-Meteo, NOAA Tides & Currents |

## Prerequisites

1. **Python 3.11+** — [python.org](https://python.org)
2. **Node.js 18+** — [nodejs.org](https://nodejs.org)
3. **PostgreSQL 15+** with PostGIS — [postgresql.org](https://postgresql.org)
4. **Redis** — Use [Memurai](https://www.memurai.com/) for Windows
5. **AISStream API Key** — Free at [aisstream.io](https://aisstream.io)

## Quick Start

```bash
# 1. Run setup
setup.bat

# 2. Configure database
# In psql:
CREATE DATABASE seatracker;
\c seatracker
CREATE EXTENSION postgis;

# 3. Edit backend/.env with your API keys

# 4. Start everything
start.bat
```

- **Frontend**: http://localhost:5173
- **Backend API**: http://localhost:8000
- **API Docs**: http://localhost:8000/docs

## Project Structure

```
Ship Tracker/
├── backend/
│   ├── main.py              # FastAPI entry point
│   ├── config.py            # Pydantic settings
│   ├── database.py          # Async SQLAlchemy + PostGIS
│   ├── models/              # ORM models (Vessel, Port, Incident, Alert, Zone)
│   ├── schemas/             # Pydantic v2 API schemas
│   ├── services/
│   │   ├── sources/         # AIS data clients (AISStream, Kystverket, NOAA, GFW)
│   │   ├── ais_aggregator.py
│   │   ├── vessel_tracker.py
│   │   ├── collision_detector.py
│   │   ├── incident_detector.py
│   │   ├── alert_engine.py
│   │   ├── zone_monitor.py
│   │   ├── weather_service.py
│   │   └── analytics_service.py
│   ├── tasks/               # Celery periodic tasks
│   ├── websocket/           # WebSocket handlers
│   ├── routers/             # REST API endpoints
│   └── utils/               # Geo, AIS, formatting utilities
├── frontend/
│   ├── src/
│   │   ├── components/      # React components (Map, Navbar, VesselPanel)
│   │   ├── hooks/           # Custom hooks (WebSocket, vessels, alerts)
│   │   ├── store/           # Zustand state stores
│   │   ├── pages/           # Route pages (Map, Dashboard, History, etc.)
│   │   └── utils/           # Frontend utilities
│   └── index.html
├── setup.bat                # One-click setup
├── start.bat                # Launch all services
└── stop.bat                 # Stop all services
```

## API Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /api/vessels` | List active vessels |
| `GET /api/vessels/search?q=` | Search vessels |
| `GET /api/vessels/{mmsi}` | Vessel details |
| `GET /api/history/{mmsi}` | Position history |
| `GET /api/incidents` | Active incidents |
| `GET /api/alerts` | Alert feed |
| `GET /api/ports` | World ports |
| `GET /api/zones` | Monitoring zones |
| `GET /api/analytics/dashboard` | Dashboard stats |
| `GET /api/weather/grid` | Weather layer data |
| `WS /ws/vessels` | Live vessel stream |
| `WS /ws/alerts` | Live alert stream |
| `WS /ws/incidents` | Live incident stream |

## License

MIT
