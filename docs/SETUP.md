# Dokploy setup guide

Follow these steps to deploy the MTProto and SOCKS5 proxies on Dokploy.

## 1) Prepare the repository

1. Fork or clone this repository.
2. Copy `.env.example` to `.env` and set strong credentials.

```bash
cp .env.example .env
```

## 2) Create a project in Dokploy

1. Open Dokploy and create a new **Project**.
2. Choose **Compose** as the project type.
3. Select the GitHub repository.
4. Confirm Dokploy will use the `docker-compose.yml` and `.env` file.

## 3) Review ports and conflicts

- MTProto defaults to **8443** on the host (container port **443**).
- SOCKS5 defaults to **1080** on the host.

If **8443** is already in use or you must use another port, update `MTPROTO_PORT` in `.env`:

```bash
MTPROTO_PORT=9443
```

## 4) Ensure the network exists

This stack attaches to the external Docker network named `dokploy-network`.

If it does not exist, create it on the host:

```bash
docker network create dokploy-network
```

## 5) Apply firewall rules

Allow only the required ports, and restrict SOCKS5 access when possible.

- Open **8443/tcp** (or your chosen MTProto port).
- Open **1080/tcp** only to trusted IPs.

Example (UFW):

```bash
ufw allow 8443/tcp
ufw allow from <YOUR_IP> to any port 1080 proto tcp
```

## 6) Deploy and verify

Deploy the project in Dokploy, then verify:

- MTProto: check logs for the `tg://` or `proxy` link.
- SOCKS5: test authentication with the credentials you set.
