# weather-service

`weather-service` is a FastAPI container that fetches daily forecasts from [Open-Meteo](https://open-meteo.com) and sends them through the internal `notifier-gateway`. 
It also exposes endpoints for direct queries.

## Features
- Scheduled weather posts (cron syntax, default 7 AM)
- `/today` and `/health` endpoints for queries
- Supports custom city, timezone, and coordinates
- Pushes notifications with `to` and `token` fields
- Designed for use within the `assistant-net` automation stack

## Environment
| Variable | Description | Example |
|-----------|-------------|----------|
| `CITY` | Default city name | `Orlando` |
| `STATE` | State or region | `FL` |
| `LAT` / `LON` | Optional coordinates to skip geocoding | `28.5383` / `-81.3792` |
| `TZ` | Timezone | `America/New_York` |
| `CRON_SCHEDULE` | When to send | `0 7 * * *` |
| `NOTIFY_URL` | Gateway endpoint | `http://notifier-gateway:8787/send` |
| `NOTIFY_TO` | Recipient phone or user ID | `+15555551234` |
| `NOTIFY_TOKEN` | Auth token, optional | `changeme` |
| `SOURCE_NAME` | Identifier for the sender | `weather-service` |

## Example Compose Service
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

## Endpoints
- `GET /health` → Service config and status  
- `GET /today?city=Orlando&state=FL` → Returns forecast JSON

Example:
```bash
curl "http://localhost:8789/today?city=Orlando&state=FL"
```

## Run Locally
```bash
cp .env.example .env
docker compose build weather-service
docker compose up weather-service
```

The service prints `[notify] 200 OK` on successful sends.

## Credits
- [Open-Meteo](https://open-meteo.com)
- [FastAPI](https://fastapi.tiangolo.com/)
- [croniter](https://pypi.org/project/croniter/)
- [pytz](https://pypi.org/project/pytz/)
