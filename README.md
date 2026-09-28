# Map Services Stack

[![GitHub License](https://img.shields.io/github/license/ndenissov/maptiler?style=for-the-badge)](https://github.com/ndenissov/maptiler/blob/main/LICENSE)
[![Docker Ready](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Nginx Proxy](https://img.shields.io/badge/Nginx-Proxy-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.org/)
[![MapProxy](https://img.shields.io/badge/MapProxy-Enabled-538b25?style=for-the-badge)](https://mapproxy.org/)
[![TileServer GL](https://img.shields.io/badge/TileServer_GL-Powered-0078FF?style=for-the-badge)](https://maptiler.com/)
[![GitHub Stars](https://img.shields.io/github/stars/ndenissov/maptiler?style=for-the-badge&logo=github&color=F3DF26)](https://github.com/ndenissov/maptiler/stargazers)

A containerized infrastructure for serving and caching map data (tiles). This project unifies local map hosting and
external source proxying through a single entry point.

## Architecture

The stack consists of three main services:

* **Map Router (Nginx)** — The single entry point (port `80`) that routes incoming requests to the appropriate backend
  services.
* **MapProxy** — A proxy server for caching external tile layers (e.g., satellite imagery). It serves tiles via the WMTS
  protocol, caching them on the fly in WebP format to save storage space.
* **TileServer GL** — A server for hosting local vector and raster tiles (e.g., OSM data).

## Project Structure

```text
.
├── config/
│   ├── mapproxy.yaml        # Caching, grids (EPSG:3857), and data sources configuration
│   └── nginx_internal.conf  # Nginx routing rules
├── docker-compose.yaml      # Docker services definition
└── LICENSE                  # Legal agreement and terms of use
```

## Prerequisites

* Docker
* Docker Compose

Before starting, ensure that the host paths mounted in the `docker-compose.yaml` volumes (such as `/opt/maps/config/`,
`/data/external/`, `/data/osm_data/`) actually exist on your host machine and contain the necessary configuration files
and map data. Alternatively, update the paths in the `volumes` section to match your system environment.

## Quick Start

1. Clone the repository and navigate to the project directory:
   ```bash
   git clone https://git.dep.ovh/ppa/maps-server.git
   cd maps-server
   ```

2. Start the services in the background:
   ```bash
   docker compose up -d
   ```

3. Check the status of the containers:
   ```bash
   docker compose ps
   ```

After a successful startup, the unified map API will be available at `http://localhost` (or your server's IP address).
Cached satellite imagery from MapProxy will be accessible via the endpoints defined in the Nginx configuration.

## Stopping the Services

To stop and remove the containers, run:

```bash
docker compose down
```

