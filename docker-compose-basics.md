# Docker Compose Basics

Notes for small multi-service setups.

## Minimal compose file

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
    restart: unless-stopped

  app:
    build: .
    environment:
      - APP_ENV=dev
    depends_on:
      - web
```

## Common commands

```bash
docker compose up -d
docker compose ps
docker compose logs -f app
docker compose exec app sh
docker compose down
```

## Ops checklist

- Pin image tags in production
- Prefer named volumes for stateful data
- Keep secrets out of compose files when possible
- Use healthchecks for dependent services
