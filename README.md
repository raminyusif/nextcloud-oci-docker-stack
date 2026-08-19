# Self-Hosted Nextcloud Stack (Oracle Cloud Infrastructure)

A private cloud storage solution built as a home lab project, deployed on an
Ubuntu VM running on Oracle Cloud Infrastructure (OCI). The stack uses Docker
Compose to run Nextcloud, a MariaDB backend, and Caddy as a reverse proxy with
automatic HTTPS via Let's Encrypt.

## Architecture

```
Internet → Caddy (reverse proxy, auto HTTPS) → Nextcloud → MariaDB
```

- **Nextcloud** — self-hosted file storage and sync, similar to Google Drive/Dropbox
- **MariaDB** — relational database backend for Nextcloud
- **Caddy** — reverse proxy that automatically provisions and renews TLS
  certificates via Let's Encrypt

## Technologies

Oracle Cloud Infrastructure, Ubuntu Linux, Docker, Docker Compose, Nextcloud,
MariaDB, Caddy, Let's Encrypt

## Features

- Automatic HTTPS with zero manual certificate management
- Persistent storage using Docker volumes (survives container restarts/updates)
- Isolated Docker bridge network between services
- HSTS header enabled for stronger transport security
- CardDAV/CalDAV redirects for contact and calendar sync clients

## Setup

1. Clone this repo onto your server.
2. Copy `.env.example` to `.env` and fill in real database credentials:
   ```bash
   cp .env.example .env
   ```
3. Edit `Caddyfile` and `docker-compose.yml` — replace `your-domain.com` and
   `your-email@example.com` with your actual domain and email.
4. Make sure DNS for your domain points to the server's public IP.
5. Start the stack:
   ```bash
   docker compose up -d
   ```
6. Visit `https://your-domain.com` and complete the Nextcloud setup wizard.

## Notes

- Ports 80/443 must be open on the server/firewall for Let's Encrypt HTTP-01
  challenge and normal HTTPS traffic.
- Database credentials and domain are excluded from version control via
  `.gitignore` / `.env.example` — never commit real secrets.
