# Traefik Reverse Proxy

Production-ready Traefik v3 reverse proxy with automatic SSL certificates.

## Features

- Automatic HTTPS with Let's Encrypt
- HTTP to HTTPS redirect
- Docker provider (auto-discovery)
- Security headers
- Rate limiting
- IP whitelisting
- Gzip compression
- Dashboard with basic auth
- Cloudflare DNS challenge support (for wildcards)

## Quick Start

```bash
# Create external network
docker network create proxy

# Configure
cp .env.example .env
nano .env

# Start
docker-compose up -d

# Test with whoami service
docker-compose --profile test up -d
```

## Access

- Dashboard: `http://traefik.yourdomain.com:8080`
- Whoami test: `https://whoami.yourdomain.com`

## Adding Services

Add these labels to your service:

```yaml
services:
  myapp:
    image: myapp:latest
    networks:
      - proxy
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.myapp.rule=Host(`myapp.example.com`)"
      - "traefik.http.routers.myapp.entrypoints=websecure"
      - "traefik.http.routers.myapp.tls.certresolver=letsencrypt"
      - "traefik.http.services.myapp.loadbalancer.server.port=8080"
      # Optional: add middleware chain
      - "traefik.http.routers.myapp.middlewares=web-chain@file"

networks:
  proxy:
    external: true
```

## Middlewares

Pre-configured middlewares in `traefik/dynamic/middlewares.yml`:

| Middleware | Description |
|------------|-------------|
| `security-headers` | XSS, CSP, HSTS headers |
| `rate-limit` | 100 req/s with burst 50 |
| `ip-whitelist` | Private networks only |
| `compress` | Gzip compression |
| `www-redirect` | www → non-www |
| `auth` | Basic authentication |
| `web-chain` | Combined: headers + rate + compress |
| `api-chain` | Combined: rate + compress |

Usage:
```yaml
labels:
  - "traefik.http.routers.myapp.middlewares=web-chain@file"
```

## SSL Certificates

### HTTP Challenge (default)
Works automatically for single domains.

### DNS Challenge (wildcards)
For `*.example.com`:

1. Set Cloudflare credentials in `.env`:
```
CF_API_EMAIL=your@email.com
CF_DNS_API_TOKEN=your-api-token
```

2. Use `cloudflare` resolver:
```yaml
labels:
  - "traefik.http.routers.myapp.tls.certresolver=cloudflare"
```

## Directory Structure

```
traefik/
├── docker-compose.yml
├── .env.example
├── traefik/
│   ├── traefik.yml          # Static config
│   └── dynamic/
│       └── middlewares.yml  # Dynamic config
└── certs/                   # SSL certificates (auto-generated)
```

## Production Checklist

- [ ] Disable dashboard or secure with strong password
- [ ] Set `api.insecure: false` in traefik.yml
- [ ] Configure proper email in ACME
- [ ] Review rate limits
- [ ] Enable access logs
- [ ] Setup log rotation

## Useful Commands

```bash
# View logs
docker-compose logs -f traefik

# Reload config (dynamic only)
# Traefik watches files automatically

# Check certificates
ls -la certs/

# Debug routing
curl -H Host:myapp.example.com http://localhost
```

## Author

Mikhail Miasnikou — System Administrator / DevOps Engineer
