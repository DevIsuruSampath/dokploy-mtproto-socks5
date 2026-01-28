# Security best practices

Follow these recommendations to keep your proxy stack secure.

## Strong credentials

- Use a **long, random password** for `SOCKS5_PASS`.
- Avoid short or reused credentials.

Example generation:

```bash
openssl rand -base64 32
```

## Rotate credentials safely

1. Update `.env` with a new `SOCKS5_USER`/`SOCKS5_PASS`.
2. Redeploy the stack in Dokploy.
3. Update client configurations.

> Tip: Rotate on a regular schedule or immediately after suspected compromise.

## Limit ingress with firewall rules

- Expose only the required ports.
- Restrict SOCKS5 to trusted IPs when possible.

Example (UFW):

```bash
ufw allow 8443/tcp
ufw allow from <YOUR_IP> to any port 1080 proto tcp
```

## Rate limiting / abuse prevention

- Consider host-level protections like **fail2ban** or **crowdsec**.
- Monitor logs for repeated authentication failures.

## Protect your control plane

- Avoid exposing the Dokploy admin UI publicly if possible.
- Enable MFA and IP allowlists where available.

## Keep images updated

- Periodically redeploy to pull the latest container images.
- Monitor upstream repositories for security advisories.

## Never commit secrets

- Do not commit `.env` files to Git.
- Keep secrets in your deployment environment only.
