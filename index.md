+++
title = "Building a Signal-Driven Weather Microservice"
date = "2025-10-27"
description = "A lightweight FastAPI container that posts daily weather updates through the assistant framework."
author = "Justin Napolitano"
taxonomies.categories = ["projects"]
taxonomies.tags = ["fastapi","docker","automation","signal","microservices","open-meteo"]
[extra]
toc = true
reaction = false
+++

## Overview
`weather-service` is a FastAPI microservice that retrieves weather data from [Open-Meteo](https://open-meteo.com) and sends daily forecasts through the notifier gateway. 
It exposes two endpoints—`/today` and `/health`—for both scheduled and on-demand use. 
This container is part of a modular automation stack built around Signal and the assistant-core.

## Features
- Scheduled forecasts via cron (`0 7 * * *` by default)
- `/today?city=Orlando&state=FL` returns a text summary and raw forecast data
- Timezone-aware through `TZ`
- Sends notifications through `notifier-gateway`
- Runs inside the `assistant-net` Docker network

## Quick Start
Add the service to your Compose stack:
```yaml
weather-service:
  build: ./weather-service
  restart: unless-stopped
  env_file: [.env]
  environment:
    CRON_SCHEDULE: "0 7 * * *"
    NOTIFY_URL: "http://notifier-gateway:8787/send"
    NOTIFY_TO: "+15555551234"
    TZ: "America/New_York"
    CITY: "Orlando"
    STATE: "FL"
    LAT: 28.5383
    LON: -81.3792
  depends_on: [notifier-gateway]
  ports: ["127.0.0.1:8789:8789"]
  networks: [assistant-net]
```

Copy the environment file and build:
```bash
cp .env.example .env
docker compose build weather-service
docker compose up -d weather-service
```

## API
| Method | Endpoint | Description |
|--------:|:----------|:-------------|
| `GET` | `/health` | Service configuration and status |
| `GET` | `/today?city=&state=` | Returns the current forecast |

Example:
```bash
curl "http://localhost:8789/today?city=Orlando&state=FL"
```

Response:
```json
{
  "city": "Orlando, Florida, United States",
  "message": "Good morning! Orlando forecast for Monday, Oct 27\nClear\nHigh 85° / Low 67° • 💧 10%",
  "raw": { ... }
}
```

## Operation
At startup, a scheduler computes the next run time based on `CRON_SCHEDULE`. 
It triggers once immediately and then at the configured interval, sending forecasts to the notifier gateway. 
The `/today` endpoint uses the same logic for real-time access.

## Integration
This service can be polled by `assistant-core` or other tools to generate daily summaries or respond to `/weather` commands in Signal. 
It can also feed structured data into an LLM for multi-source morning briefs.

## Links
- Source: [GitHub → weather-service](https://github.com/justin-napolitano/weather-service)
- API Docs: `http://localhost:8789/docs`
- Related: `assistant-core`, `notifier-gateway`, `signal-api`
