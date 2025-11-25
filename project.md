---
slug: github-weather-servb
id: github-weather-servb
title: Python FastAPI Weather Service with Notifications
repo: justin-napolitano/weather-servb
githubUrl: https://github.com/justin-napolitano/weather-servb
generatedAt: '2025-11-24T21:36:48.492Z'
source: github-auto
summary: >-
  Explore a Python FastAPI microservice for daily weather forecasts, featuring REST API endpoints
  and scheduled notifications.
tags:
  - fastapi
  - python
  - docker
  - cron
  - uvicorn
  - pytz
seoPrimaryKeyword: python fastapi weather service
seoSecondaryKeywords:
  - weather notifications
  - rest api endpoints
  - scheduled updates
  - timezone handling
  - docker deployment
seoOptimized: true
topicFamily: automation
topicFamilyConfidence: 0.9
kind: project
entryLayout: project
showInProjects: true
showInNotes: false
showInWriting: false
showInLogs: false
---

`weather-service` is a Python-based FastAPI microservice designed to provide daily weather forecasts and send notifications through an internal gateway. It supports scheduled updates, direct query endpoints, and flexible location and timezone configurations.

## Features

- Scheduled daily weather notifications using cron syntax (default schedule: 7:00 AM local time)
- REST API endpoints:
  - `GET /health` for service status and configuration
  - `GET /today?city=City&state=State` for current weather forecast
- Supports specifying location by city/state or directly via latitude and longitude
- Timezone-aware scheduling and responses
- Pushes notifications to a configurable notifier gateway with optional authentication
- Containerized with Docker for straightforward deployment

## Tech Stack

- Python 3.12
- FastAPI for web framework
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
git clone https://github.com/justin-napolitano/weather-service.git
cd weather-service
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
docker build -t weather-service .
docker run -p 8789:8789 --env-file .env weather-service
```

Alternatively, if using Docker Compose:

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
/app.py           # Main application entry point with API and scheduling logic
/Dockerfile       # Docker container definition
/README.md        # This documentation file
/requirements.txt # Python dependencies
/weather.py       # Weather fetching utility module
```

## Future Work / Roadmap

- Add support for more detailed weather data and forecasts
- Implement caching to reduce external API calls
- Expand notification channels beyond the current gateway
- Add authentication and rate limiting for API endpoints
- Improve error handling and logging
- Provide Helm charts or Kubernetes manifests for cloud deployment


