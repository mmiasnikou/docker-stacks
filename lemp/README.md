# LEMP Stack

Production-ready LEMP stack (Linux, Nginx, MySQL, PHP) with Redis caching.

## Components

| Service | Version | Port |
|---------|---------|------|
| Nginx | Alpine | 80, 443 |
| PHP-FPM | 8.2 | 9000 |
| MySQL | 8.0 | 3306 |
| Redis | Alpine | 6379 |
| phpMyAdmin | Latest | 8080 |

## Features

- PHP 8.2 with essential extensions (pdo, gd, redis, opcache, etc.)
- Redis for session storage and caching
- OPcache enabled for performance
- Gzip compression
- Security headers configured
- Persistent volumes for data
- phpMyAdmin (optional, via profile)

## Quick Start

```bash
# Clone and configure
cp .env.example .env
nano .env  # Set your passwords

# Start stack
docker-compose up -d

# With phpMyAdmin
docker-compose --profile tools up -d

# View logs
docker-compose logs -f

# Stop
docker-compose down
```

## Directory Structure

```
lemp/
├── docker-compose.yml
├── .env.example
├── nginx/
│   └── conf.d/
│       └── default.conf
├── php/
│   ├── Dockerfile
│   └── php.ini
├── mysql/
│   ├── conf.d/
│   └── init/
├── www/
│   └── public/
└── logs/
```

## Configuration

### Add SSL

1. Place certificates in `nginx/ssl/`
2. Update `nginx/conf.d/default.conf`:

```nginx
server {
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;
    # ...
}
```

### Custom PHP Extensions

Edit `php/Dockerfile` and rebuild:

```bash
docker-compose build php
docker-compose up -d
```

### MySQL Init Scripts

Place `.sql` files in `mysql/init/` — they run on first start.

## Useful Commands

```bash
# PHP container shell
docker exec -it php sh

# MySQL CLI
docker exec -it mysql mysql -u root -p

# Redis CLI
docker exec -it redis redis-cli

# Check PHP extensions
docker exec php php -m

# Composer install
docker exec -w /var/www/html php composer install
```

## Author

Mikhail Miasnikou — System Administrator / DevOps Engineer
