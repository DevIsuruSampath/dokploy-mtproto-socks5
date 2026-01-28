# dokploy-proxy-stack

Production-ready Docker Compose stack for Dokploy that deploys:

- **MTProto proxy** (Telegram MTProxy) via `telegrammessenger/proxy:latest`
- **SOCKS5 proxy** via `ghcr.io/tarampampam/3proxy:1`

This stack is built for raw TCP proxying (no Traefik labels) and attaches to the external Docker network `dokploy-network`.

## Quick start (Dokploy)

1. **Clone this repo**
   ```bash
   git clone <your-repo-url>
   cd dokploy-proxy-stack
   ```
2. **Create your environment file**
   ```bash
   cp .env.example .env
   ```
3. **Set strong credentials** in `.env` (especially `SOCKS5_USER`/`SOCKS5_PASS`).
4. **Deploy in Dokploy**
   - Create a Project → Compose → select this repository.
   - Dokploy will read `docker-compose.yml` and `.env`.

## Local Docker Compose usage (optional)

If you want to test locally:

```bash
cp .env.example .env
docker compose up -d
```

> **Note:** The Dokploy workflow is the recommended deployment path.

## MTProto: getting the secret and Telegram link

The MTProto image prints the proxy secret and link in container logs on first startup. View the logs in Dokploy or with:

```bash
docker logs <mtproto-container-id>
```

Example output (format will match this):

```
TG://proxy?server=<YOUR_IP>&port=8443&secret=<SECRET>
```

Add the proxy in Telegram:

- **Mobile:** Settings → Data and Storage → Proxy → Add Proxy → MTProto
- **Desktop:** Settings → Advanced → Connection Type → Use Custom Proxy → MTProto

Paste the **server**, **port**, and **secret** values from the log output.

## SOCKS5 usage

Use the following values (from your `.env`):

- **Host:** your server IP or DNS name
- **Port:** `1080` by default (`SOCKS5_PORT`)
- **Username:** `SOCKS5_USER`
- **Password:** `SOCKS5_PASS`

## Default ports

- MTProto host port: **8443** → container **443**
- SOCKS5 host port: **1080** → container **1080**

Adjust in `.env` if you have conflicts.

## Security and legal use

These proxies can be abused if exposed publicly without controls. Use strong credentials, limit ingress with firewall rules, and rotate secrets regularly. See the [security guide](docs/SECURITY.md) for best practices.

**Use these services only for lawful, authorized traffic.**
