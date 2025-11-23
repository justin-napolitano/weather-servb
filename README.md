# weather-servb

`weather-servb` is a Python-based FastAPI service designed to fetch daily weather forecasts from Open-Meteo and deliver notifications through an internal gateway. It supports scheduled posts, direct query endpoints, and configurable location and timezone settings.

## Features

- Scheduled daily weather notifications using cron syntax (default at 7:00 AM local time)
- REST API endpoints:
  - `GET /health` for service status and configuration
  - `GET /today?city=City&state=State` for current forecast data
- Supports specifying city, state, timezone, or direct geographic coordinates (latitude and longitude)
- Pushes notifications via a configurable notifier gateway with authentication support
- Containerized with Docker for easy deployment

## Tech Stack

- Python 3.12
- FastAPI for the web framework
- Uvicorn as ASGI server
- Requests for HTTP calls
- croniter for cron schedule parsing
- pytz for timezone handling
- Docker for containerization

## Getting Started

### Prerequisites

- Docker and Docker Compose installed

### Installation & Run

1. Clone the repository:

```bash
git clone https://github.com/justin-napolitano/weather-servb.git
cd weather-servb
```

2. Create an `.env` file or set environment variables as needed. Example variables:

```
CITY=Orlando
STATE=FL
TZ=America/New_York
CRON_SCHEDULE="0 7 * * *"
NOTIFY_URL=http://notifier-gateway:8787/send
NOTIFY_TO=+15555551234
NOTIFY_TOKEN=changeme
SOURCE_NAME=weather-service
LAT=28.5383
LON=-81.3792
```

3. Build and run the Docker container:

```bash
docker build -t weather-servb .
docker run -p 8789:8789 --env-file .env weather-servb
```

Alternatively, use Docker Compose if available:

```bash
docker compose build weather-service
docker compose up weather-service
```

### Usage

- Check service health:

```bash
curl http://localhost:8789/health
```

- Get today's forecast:

```bash
curl "http://localhost:8789/today?city=Orlando&state=FL"
```

## Project Structure

```
/app.py           # Main FastAPI application with scheduling and endpoints
/Dockerfile       # Container build instructions
/README.md        # Project documentation
/requirements.txt # Python dependencies
/weather.py       # Helper module for simple weather fetch via wttr.in
```

## Future Work / Roadmap

- Add support for more detailed weather data and additional forecast parameters
- Implement caching to reduce redundant external API calls
- Enhance error handling and logging
- Add unit and integration tests
- Support multiple notification channels (e.g., email, SMS, push notifications)
- Provide metrics and monitoring endpoints
- Allow configuration via a UI or API

---

*Note: This README is generated based on available source and inferred details.*