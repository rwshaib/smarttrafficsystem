# Smart Real-Time Traffic Management System Backend

## Project Overview
This project is the backend for a Smart Real-Time Traffic Management System. It receives location data, analyzes simulated nearby traffic and intersections, identifies simulated emergency events (like ambulances), and makes intelligent traffic light priority recommendations based on rule-based logic to mitigate congestion.

## Problem
Traffic congestion in metropolitan areas leads to wasted time, increased emissions, and delayed emergency responses. Without real-time data, traffic lights operate on rigid, inefficient timers. 

## Solution
This backend acts as a central decision engine. It takes live location data (simulated for now), evaluates traffic density at intersections, and recommends dynamic green light adjustments. It also features an emergency override system that instantly reprioritizes signals to clear paths for emergency vehicles.

## Architecture
```text
FUTURE FRONTEND
      |
      | latitude + longitude
      v
FASTAPI BACKEND
      |
      +--> LOCATION MODULE
      |
      +--> TRAFFIC MODULE
      |
      +--> EMERGENCY MODULE
      |
      +--> DECISION ENGINE
      |
      +--> DATABASE
      |
      v
JSON RESPONSE
```

## Technology Stack
- **Python 3.11+**: The programming language of choice for its simplicity and ecosystem.
- **FastAPI**: Used for building the API quickly, with automatic Pydantic validation and Swagger docs.
- **Uvicorn**: An ASGI web server to serve the FastAPI application efficiently.
- **SQLAlchemy & SQLite**: Used for the ORM and database. SQLite allows zero-config local development, with SQLAlchemy making it easy to migrate to PostgreSQL later.
- **Pydantic**: For data validation and schema definitions.
- **Pytest**: Used for writing robust, automated tests.

## Installation

### Windows
```powershell
python -m venv venv
.\venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env
```

### macOS / Linux
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

## Running the Backend

Run the backend server with:
```bash
uvicorn app.main:app --reload
```

- API Base URL: `http://localhost:8000`
- Swagger Interactive Documentation: `http://localhost:8000/docs`

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health` | Check if service is healthy |
| GET | `/api/v1/health` | Check if API v1 is healthy |
| POST | `/api/v1/location/analyze` | Validate location coordinates |
| POST | `/api/v1/traffic/nearby` | Get nearby simulated traffic data |
| GET | `/api/v1/traffic/intersection/{intersection_id}` | Get traffic data for an intersection |
| POST | `/api/v1/emergency/report` | Report an active emergency vehicle |
| GET | `/api/v1/emergency/nearby` | Get active emergency vehicles |
| POST | `/api/v1/recommendations/generate` | Get a recommendation for a specific intersection |
| POST | `/api/v1/dashboard/analyze` | Run full analysis flow (location, traffic, emergency, decision) |

## Example Requests

### Dashboard Analysis Request

**cURL:**
```bash
curl -X POST http://localhost:8000/api/v1/dashboard/analyze \
  -H "Content-Type: application/json" \
  -d '{"latitude": 28.6139, "longitude": 77.2090, "radius_km": 5}'
```

**JSON Body:**
```json
{
  "latitude": 28.6139,
  "longitude": 77.2090,
  "radius_km": 5
}
```

## Testing

To run the automated tests, ensure you are in your activated virtual environment and run:
```bash
pytest
```

## Simulated Data
**This hackathon prototype currently uses simulated traffic and emergency data. It does not claim that the displayed data is live real-world traffic data.**
All endpoints relying on simulations explicitly return `"data_source": "simulation"`.

## Future Integration
The current modules are designed around generic schemas. In the future:
- **Traffic APIs/Computer Vision**: The simulated data generators in `traffic_service.py` can be replaced with HTTP calls to Google Maps API, Mapbox, or local traffic cameras without changing the API contract.
- **Sensors**: IoT inductive loops or radar sensors can push data directly to the database.
- **Emergency Data**: Authorized municipal APIs can replace the simulated emergency reporting.

## GitHub Preparation
When pushing to GitHub, you only need to commit the code and configuration files.

Do not commit:
- `.env`
- `venv/`
- `__pycache__/`
- `*.db` (local database files)
