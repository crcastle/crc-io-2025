---
draft: true
slug: deploy-pocket-id-fly-io
# description: "-"
title: Deploy Pocket ID on Fly.io
date: 2025-06-01
tags:
  - technical
  - learning
---

TODO: why: cost, simplicity, don't have to run my own proxy server

We're going to convert the content in Pocket ID's [`docker-compose.yml`](https://github.com/pocket-id/pocket-id/blob/main/docker-compose.yml) to Fly.io's [`fly.toml`](https://fly.io/docs/reference/configuration/). Fly.io doesn't provide an automated way to deploy using or convert a `docker-compose.yml` file. As of May 2025, here's what the `docker-compose.yml` looks like:

```yaml
services:
  pocket-id:
    image: ghcr.io/pocket-id/pocket-id
    restart: unless-stopped
    env_file: .env
    ports:
      - 1411:1411
    volumes:
      - "./data:/app/data"
    # Optional healthcheck
    healthcheck:
      test: "curl -f http://localhost:1411/healthz"
      interval: 1m30s
      timeout: 5s
      retries: 2
      start_period: 10s
```

1. Clone git@github.com:pocket-id/pocket-id.git

- Pocket ID's default database is SQLite, which works great on fly.io. I'm using just a tiny 2GB volume to persist the database data between deploys.