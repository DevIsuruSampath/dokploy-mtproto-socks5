# Troubleshooting

## MTProto not connecting / secret missing

- The MTProto image prints the secret on first startup. Check logs in Dokploy or:
  ```bash
  docker logs <mtproto-container-id>
  ```
- If you restarted the container, the secret should persist in the `mtproto_data` volume.
- Ensure the MTProto host port is reachable from the internet.

## Port 443/8443 already in use

- Change the host port by updating `MTPROTO_PORT` in `.env`:
  ```bash
  MTPROTO_PORT=9443
  ```
- Redeploy in Dokploy after changing `.env`.

## SOCKS5 authentication fails

- Confirm the `SOCKS5_USER` and `SOCKS5_PASS` values in `.env`.
- Redeploy after updating credentials.
- Verify clients are using the correct host, port, username, and password.

## Network "dokploy-network" missing

This stack requires the external network `dokploy-network`.

Create it if absent:

```bash
docker network create dokploy-network
```

## Viewing logs

- **Dokploy:** use the project logs UI.
- **CLI:**
  ```bash
  docker logs <container-id>
  ```
