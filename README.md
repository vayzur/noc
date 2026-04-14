# KEPLER

Website and service connectivity monitoring with Gatus.

This repository contains a Gatus setup and a curated list of endpoints to monitor from a specific network.

The inventory is grouped across:

- Popular Websites
- AI
- Frameworks
- Code & Dev
- Research
- Packages
- Cloud & Hosting
- Data Platforms
- Runtimes & OS
- DevOps
- Security

## Stack

- Gatus for HTTP monitoring and dashboard
- Docker Compose for deployment
- SQLite for local persistence
- Host-level nginx can be added later for reverse proxy and TLS

## Repository Layout

```text
.
├── compose.yaml
├── config/
│   ├── 00-base.yaml
│   ├── 05-popular-websites.yaml
│   ├── 10-ai.yaml
│   ├── 15-frameworks.yaml
│   ├── 20-code-dev.yaml
│   ├── 25-research.yaml
│   ├── 30-package-registries.yaml
│   ├── 40-cloud-hosting.yaml
│   ├── 50-data-platforms.yaml
│   ├── 60-runtimes-and-os.yaml
│   ├── 70-devops-and-observability.yaml
│   └── 80-security-and-identity.yaml
└── data/
```

## Deploy

Requirements:

- Docker Engine
- Docker Compose

Run:

```bash
docker compose up -d
```

Check status:

```bash
docker compose ps
docker compose logs --tail=100 gatus
```

Test locally on the host:

```bash
curl http://127.0.0.1:8080
```

## Deployment Model

This repo is intended for a host-based deployment model:

- Gatus runs in Docker
- nginx can run directly on the host later
- TLS can be terminated on the host later

Because Gatus is bound to `127.0.0.1:8080`, it is not exposed publicly until you choose to proxy it.

## Configuration

The base application settings live in [config/00-base.yaml](./config/00-base.yaml).

Important details:

- `GATUS_CONFIG_PATH=/config`
- config is mounted read-only into the container
- Gatus merges all `*.yaml` and `*.yml` files under `config/`
- state is stored in `./data/gatus.db`

## Inventory Notes

The target list is intentionally biased toward commonly used services, developer tooling, infrastructure sites, package registries, research platforms, and core operating system websites.

Redundant checks are mostly avoided:

- if the main domain is enough, subdomains are usually not monitored
- product-specific subdomains are kept only when they are meaningfully separate services

## Notes

- If you want app-level auth later, add `security.basic` to `config/00-base.yaml`
- If you want public access later, put a dedicated hostname in front of `127.0.0.1:8080`
- Back up both `config/` and `data/`
- Keep the Docker log rotation block in `compose.yaml` unless you already manage container logs elsewhere
