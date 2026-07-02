# Replication Checklist: Hermes Podman Instance (Mark 1)
Date: 2026-06-13
Protocol: 3-Turn (PLAN/EXECUTE/DEBRIEF)

## 1. Host Preparation
- [ ] Verify `podman` installation.
  - Install if missing: `apt-get update && apt-get install -y podman podman-compose`
- [ ] Create dedicated network:
  ```bash
  podman network create hermes-net
  ```
- [ ] Create persistent volume:
  ```bash
  podman volume create hermes-data
  ```

## 2. Image Procurement
- [ ] Preferred: `ghcr.io/nousresearch/hermes-agent:latest`
- [ ] Fallback: `docker.io/nousresearch/hermes-agent:latest` (use if GHCR auth fails)

## 3. Deployment (Compose)
Create `docker-compose.yml` in a dedicated directory:
```yaml
version: '3.8'
services:
  hermes-instance:
    image: docker.io/nousresearch/hermes-agent:latest
    container_name: hermes-agent-replication
    networks:
      - hermes-net
    volumes:
      - hermes-data:/root/.hermes
    env_file:
      - .env
    restart: always

networks:
  hermes-net:
    external: true

volumes:
  hermes-data:
    external: true
```

## 4. Identity & Gateway
- [ ] **Crucial:** Create a unique Discord Bot Token for the new instance.
- [ ] **Constraint:** Never reuse the same token for two active instances (causes heartbeat/reconnect loops).
- [ ] Populate `.env` with unique `DISCORD_TOKEN` and shared `OPENROUTER_API_KEY`.

## 5. Launch
```bash
podman-compose up -d
```
