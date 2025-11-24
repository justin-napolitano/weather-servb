---
slug: github-weather-servb-writing-overview
id: github-weather-servb-writing-overview
title: 'Weather-servb: Your Local Weather Forecaster'
repo: justin-napolitano/weather-servb
githubUrl: https://github.com/justin-napolitano/weather-servb
generatedAt: '2025-11-24T18:12:12.730Z'
source: github-auto
summary: >-
  I built **weather-servb** to solve a simple problem: receiving timely weather
  updates tailored to my needs. This Python-based microservice is quick, handy,
  and incredibly easy to deploy. Let's dive in.
tags: []
seoPrimaryKeyword: ''
seoSecondaryKeywords: []
seoOptimized: false
topicFamily: null
topicFamilyConfidence: null
kind: writing
entryLayout: writing
showInProjects: false
showInNotes: false
showInWriting: true
showInLogs: false
---

I built **weather-servb** to solve a simple problem: receiving timely weather updates tailored to my needs. This Python-based microservice is quick, handy, and incredibly easy to deploy. Let's dive in.

## What Is Weather-servb?

At its core, weather-servb is a FastAPI microservice that delivers daily weather forecasts. It goes a step further by sending notifications via a configurable internal notifier gateway. Think of it as your personal weather assistant that nudges you when it’s time to grab an umbrella or enjoy that sunny morning.

### Why I Built It

The inspiration came from my own frustrations with existing weather services. I wanted something straightforward, especially for scheduling notifications and being timezone-aware. I needed flexibility in querying weather data for different locations, so I decided to build my own.

## Key Features

Here are some standout features:

- **Scheduled Notifications**: Daily weather updates with a default schedule at 7 AM local time, customizable via cron syntax.
- **REST API**: 
  - `GET /health` checks the status of the service.
  - `GET /today?city=City&state=State` fetches the current weather forecast for the specified location.
- **Location Flexibility**: You can specify the location either by city/state or through latitude and longitude.
- **Timezone Awareness**: All responses are delivered with proper timezone context.
- **Notification Gateway**: Pushes weather alerts to a notifier gateway while supporting optional authentication.
- **Containerized Deployment**: Built with Docker, making it a breeze to deploy anywhere.

## Tech Stack

Here's a look at the stack that powers weather-servb:

- **Python 3.12**: The latest version for performance and modern features.
- **FastAPI**: For a speedy, user-friendly API.
- **Uvicorn**: As a lightweight ASGI server handling requests.
- **Requests**: To manage HTTP calls when fetching weather data.
- **croniter**: To parse cron schedules effortlessly.
- **pytz**: For managing timezones.
- **Docker**: Containerizing the whole service for seamless deployment.

## Getting Started

### Prerequisites

Make sure you have Docker and Docker Compose installed. If not, get those set up first.

### Installation & Run

1. Clone my repo:

   ```bash
   git clone https://github.com/justin-napolitano/weather-servb.git
   cd weather-servb
   ```

2. Configure your environment. Create an `.env` file simulating this setup:

   ```
   CITY=Orlando
   STATE=FL
   TZ=America/New_York
   CRON_SCHEDULE="0 7 * * *"
   NOTIFY_URL=http://notifier-gateway:8787/send
   NOTIFY_TO=+15555551234
   NOTIFY_TOKEN=changeme
   SOURCE_NAME=weather-servb
   LAT=28.5383
   LON=-81.3792
   ```

3. Build and run:

   ```bash
   docker build -t weather-servb .
   docker run -p 8789:8789 --env-file .env weather-servb
   ```

   Or if you prefer Docker Compose:

   ```bash
   docker compose build weather-servb
   docker compose up weather-servb
   ```

### Usage

To check the service health:

```bash
curl http://localhost:8789/health
```

To get today's weather forecast:

```bash
curl "http://localhost:8789/today?city=Orlando&state=FL"
```

## Project Structure

Here's a quick overview of how the project is organized:

```
/app.py           # Main entry coordinating API and scheduling
/Dockerfile       # Docker container definition
/README.md        # Documentation
/requirements.txt # Dependencies
/weather.py       # Utility for fetching weather data
```

## Key Design Decisions

I went with FastAPI for its speed and simplicity. It allows for quick development and easy integration with Python's async capabilities. Docker makes it easy to deploy in various environments without worrying about configuration issues. 

Choosing to implement timezone-aware features was non-negotiable for me. I wanted users to receive notifications that make sense for their local setting.

## Trade-offs Made

Selecting the tech stack wasn’t without its compromises. FastAPI’s learning curve is steeper than other frameworks like Flask, but it's worth it for the performance gains. Also, Docker adds an extra layer of complexity, but for deployment, I think it pays off.

## What I’d Like to Improve Next

There's always room for improvement, right? Here's what I’m eyeing:

- **Enhanced Weather Data**: More detailed forecasts would add value.
- **Caching**: To minimize external API calls, reducing latency and potential costs.
- **More Notification Channels**: To broaden how users receive updates.
- **Authentication & Rate Limiting**: To safeguard API endpoints from misuse.
- **Improved Error Handling**: Because things do go wrong.
- **Cloud Deployment Options**: Providing Helm charts or Kubernetes manifests would make it easier for cloud adopters.

## Stay Updated

If you want to follow my journey with weather-servb and see ongoing updates, feel free to connect with me on Mastodon, Bluesky, or Twitter/X. Your feedback is always welcome!

In conclusion, weather-servb is my take on creating a simple, effective, and customizable weather notification system. I hope you'll find it as useful as I intended it to be!
