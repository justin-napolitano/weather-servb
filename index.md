---
slug: github-weather-service
title: Automated Weather Notification Service Overview
repo: justin-napolitano/weather-service
githubUrl: https://github.com/justin-napolitano/weather-service
generatedAt: '2025-11-23T09:51:41.083483Z'
source: github-auto
summary: >-
  Explore the architecture and implementation of a Python-based weather
  notification service using FastAPI and Docker.
tags:
  - python
  - fastapi
  - docker
  - microservice
  - weather-data
  - scheduling
  - cron
  - notifications
seoPrimaryKeyword: automated weather notification service
seoSecondaryKeywords:
  - weather data retrieval
  - FastAPI deployment
  - Docker containerization
  - scheduling with cron
  - timezone handling
seoOptimized: true
topicFamily: automation
topicFamilyConfidence: 0.95
topicFamilyNotes: >-
  The post describes a Python microservice for automated weather notifications
  including scheduling via cron, deployment using Docker, and an API. This
  aligns best with the Automation family which covers scripts and projects
  focused on automating deployment, scheduling, and related workflows.
kind: project
id: github-weather-service
---

# weather-service: Technical Overview and Implementation Notes

## Motivation and Problem Statement

The `weather-service` project addresses the need for an automated, reliable, and configurable weather notification system within an internal infrastructure. It solves the problem of fetching daily weather forecasts and delivering them as notifications without manual intervention. By using scheduled tasks and RESTful endpoints, it enables both push-based and pull-based access to weather data.

## Architecture and Components

The service is implemented in Python 3.12 using FastAPI as the web framework, with Uvicorn serving as the ASGI server. It is containerized via Docker for ease of deployment and environment consistency.

### Scheduling

The core scheduling mechanism relies on the `croniter` library to parse cron expressions defined by the `CRON_SCHEDULE` environment variable (defaulting to daily at 7:00 AM local time). The service uses Python's threading to run scheduled jobs asynchronously alongside the FastAPI server.

### Location and Timezone Handling

Locations can be specified either by city and state or by latitude and longitude coordinates. The service includes a mapping of US state abbreviations to full names to assist with geocoding. Timezone awareness is managed using the `pytz` library, ensuring that scheduled tasks and timestamps respect the configured timezone.

### Weather Data Retrieval

Weather data is fetched via HTTP calls. The `weather.py` module currently uses the `requests` library to obtain weather information from an external source (`wttr.in`) with a simple text format. The main application appears to integrate with Open-Meteo for more detailed forecasts, though the exact implementation details are partially truncated.

### Notification Delivery

Notifications are sent to an internal notifier gateway specified by the `NOTIFY_URL` environment variable. Authentication tokens and recipient identifiers can be configured via environment variables to secure and direct notifications appropriately.

### API Endpoints

- `/health`: Returns service status and configuration details for monitoring and debugging.
- `/today`: Accepts query parameters for city and state, returning the current weather forecast.

These endpoints allow both internal systems and users to query the service directly.

## Deployment

The Dockerfile defines a slim Python 3.12 image with minimal dependencies installed. The container exposes port 8789, matching the FastAPI server's listening port. Environment variables provide flexible configuration without code changes.

## Practical Considerations

- The service uses environment variables extensively for configuration, enabling easy adjustment in different environments.
- Error handling in the weather fetching utility is basic, returning error messages as strings; this could be improved for robustness.
- The use of threading for scheduling alongside FastAPI is a straightforward approach but may require attention to concurrency and shutdown behavior.

## Assumptions

- The notifier gateway is an internal service reachable at the configured URL.
- The external weather API usage is limited and does not require complex authentication.
- The service runs in a controlled environment where Docker is available.

## Summary

`weather-service` is a focused microservice designed for automated weather notifications with configurable scheduling and location parameters. Its architecture balances simplicity with flexibility, leveraging Python's ecosystem and containerization to facilitate deployment and integration within internal systems. Future improvements could enhance data richness, reliability, and operational features.


