# Task 7: Monitor System Resources Using Netdata

## Objective
Install **Netdata** on Windows (via Docker) and visualize system and application performance metrics.

---

## Tools Used
- **Netdata** (free, open-source monitoring tool)
- **Docker Desktop (Windows, Linux containers mode)**

---

## Steps to Run

### 1. Prerequisites
- Install [Docker Desktop](https://www.docker.com/products/docker-desktop).
- Switch Docker to **Linux containers**.
- Ensure Docker is running (`docker ps` should work).

### 2. Run with Docker Compose (recommended)
Create a file `docker-compose.yml`:

```yaml
version: "3.7"

services:
  netdata:
    image: netdata/netdata
    container_name: netdata
    restart: unless-stopped
    cap_add:
      - SYS_PTRACE
    security_opt:
      - apparmor:unconfined
    ports:
      - "19999:19999"
    volumes:
      - netdataconfig:/etc/netdata
      - netdatalib:/var/lib/netdata
      - netdatacache:/var/cache/netdata
      - /etc/passwd:/host/etc/passwd:ro
      - /etc/group:/host/etc/group:ro
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /etc/os-release:/host/etc/os-release:ro
      # Optional: Monitor Docker containers
      # - /var/run/docker.sock:/var/run/docker.sock:ro

volumes:
  netdataconfig:
  netdatalib:
  netdatacache:
