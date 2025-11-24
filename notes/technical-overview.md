---
slug: github-weather-servb-note-technical-overview
id: github-weather-servb-note-technical-overview
title: weather-servb
repo: justin-napolitano/weather-servb
githubUrl: https://github.com/justin-napolitano/weather-servb
generatedAt: '2025-11-24T18:49:23.143Z'
source: github-auto
summary: >-
  `weather-servb` is a Python FastAPI microservice that delivers daily weather
  forecasts and sends notifications via an internal gateway. It's designed for
  flexible location queries, timezone handling, and uses cron syntax for
  scheduled updates.
tags: []
seoPrimaryKeyword: ''
seoSecondaryKeywords: []
seoOptimized: false
topicFamily: null
topicFamilyConfidence: null
kind: note
entryLayout: note
showInProjects: false
showInNotes: true
showInWriting: false
showInLogs: false
---

`weather-servb` is a Python FastAPI microservice that delivers daily weather forecasts and sends notifications via an internal gateway. It's designed for flexible location queries, timezone handling, and uses cron syntax for scheduled updates.

## Key Features

- Scheduled notifications (default: 7:00 AM local)
- REST API endpoints:
  - `GET /health` checks service status
  - `GET /today?city=City&state=State` fetches today's forecast
- Location settings via city/state or lat/lon
- Sends notifications with optional authentication

## Getting Started

1. Clone the repo:
   ```bash
   git clone https://github.com/justin-napolitano/weather-servb.git
   cd weather-servb
   ```

2. Set up `.env` variables:
   ```
   CITY=Orlando
   STATE=FL
   TZ=America/New_York
   CRON_SCHEDULE="0 7 * * *"
   NOTIFY_URL=http://notifier-gateway:8787/send
   ```

3. Build and run:
   ```bash
   docker build -t weather-servb .
   docker run -p 8789:8789 --env-file .env weather-servb
   ```
   Or with Docker Compose:
   ```bash
   docker compose up
   ```

### Gotchas

Make sure the notifier gateway URL is accessible; otherwise, notifications won't go out.
