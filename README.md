# Chatwoot Self-Hosted Traefik Deployment

This repository contains a ready-to-run Docker Compose stack for deploying **Chatwoot Community Edition** integrated with **Traefik**.

## Services Included

1. **`db` (pgvector:pg17)**: High-performance PostgreSQL database.
2. **`redis` (redis:7.0)**: Queue & Cache database.
3. **`web` (chatwoot:latest)**: Main rails server running the dashboard/APIs (port 3000, exposed via Traefik).
4. **`worker` (chatwoot:latest)**: Sidekiq background job processor.

---

## Quickstart Deployment

### 1. Verify Configuration (`.env`)
A `.env` file has been automatically generated for you. Open it and customize:
*   `CHATWOOT_DOMAIN`: The domain where Chatwoot will be accessible (e.g. `chatwoot.mimbex.com`).
*   `FRONTEND_URL`: The full URL including protocol (e.g. `https://chatwoot.mimbex.com`).
*   `TRAEFIK_NETWORK`: The name of your external Traefik bridge network (defaults to `traefik-network`).

> [!NOTE]
> The `SECRET_KEY_BASE` and `POSTGRES_PASSWORD` have already been populated with secure, randomly generated values.

### 2. Verify Traefik Network
Traefik relies on a shared external network to communicate with the web container. If it doesn't exist yet, create it:
```bash
docker network create traefik-network
```

### 3. Initialize & Migrate Database
Before starting the servers, initialize the database and run the Rails migrations:
```bash
docker compose run --rm web bundle exec rails db:prepare
```

### 4. Start the Services
Run the containers in detached mode:
```bash
docker compose up -d
```

### 5. Access and Setup Admin Account
Open your browser at `https://<your_chatwoot_domain>` (or `http://chatwoot.localhost` if testing locally with custom hosts).
You will be greeted by the Chatwoot **Onboarding Signup Page** where you can create the first super-administrator account.
