# Docker Load Balancer Demo

A simple demonstration of load balancing using Docker and Caddy. Two Caddy file servers sit behind a Caddy load balancer using a round-robin strategy.

## Architecture

```
Client → Load Balancer (port 8880) → caddy1 (port 80) → caddy2 (port 80)
```

All containers communicate over a shared Docker bridge network (`caddytest`).

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)

## Setup

### 1. Create the Docker network

```bash
docker network create caddytest
```

### 2. Start the application servers

```bash
docker run -d --network caddytest --name caddy1 -v $PWD/server1/index.html:/usr/share/caddy/index.html caddy
docker run -d --network caddytest --name caddy2 -v $PWD/server2/index.html:/usr/share/caddy/index.html caddy
```

### 3. Start the load balancer

```bash
docker run -d --network caddytest -p 8880:80 -v $PWD/Caddyfile:/etc/caddy/Caddyfile caddy
```

## Usage

Hit the load balancer at http://localhost:8880/. Each request will round-robin between the two servers.

```bash
curl http://localhost:8880/
```
