# Docker Stacks

Production-ready Docker Compose configurations for common infrastructure needs.

## Stacks

### 🌐 [LEMP](./lemp/)
Full web stack: Nginx + PHP 8.2 + MySQL 8 + Redis

```bash
cd lemp && docker-compose up -d
```

### 📊 [Monitoring](./monitoring/)
Prometheus + Grafana + Alertmanager + Node Exporter + cAdvisor

```bash
cd monitoring && docker-compose up -d
```

### 🔒 [Traefik](./traefik/)
Reverse proxy with automatic SSL (Let's Encrypt)

```bash
docker network create proxy
cd traefik && docker-compose up -d
```

## Features

- Production-ready configurations
- Security best practices
- Persistent volumes
- Environment-based configuration
- Comprehensive documentation
- Telegram notifications support

## Quick Reference

| Stack | Ports | Main URL |
|-------|-------|----------|
| LEMP | 80, 443, 3306, 6379 | http://localhost |
| Monitoring | 3000, 9090, 9093 | http://localhost:3000 |
| Traefik | 80, 443, 8080 | http://localhost:8080 |

## Requirements

- Docker 20.10+
- Docker Compose 2.0+
- 2GB+ RAM (for monitoring stack)

## Usage Pattern

1. Navigate to stack directory
2. Copy and edit `.env.example` → `.env`
3. Run `docker-compose up -d`
4. Check logs: `docker-compose logs -f`

## Author

Mikhail Miasnikou — System Administrator / DevOps Engineer

## License

MIT
