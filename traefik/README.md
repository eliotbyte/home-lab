# Traefik Proxy Setup

## Prerequisites
- Docker and Docker Compose installed
- Cloudflare account with:
  - API Token (DNS:Edit permissions)
  - Tunnel Token
  - Domain configured in Cloudflare

## Setup

1. Copy environment template:
```bash
cp env.example .env
```

2. Edit `.env` with your values:
- `ACME_EMAIL` - your email for Let's Encrypt
- `ROOT_DOMAIN` - your domain
- `CF_DNS_API_TOKEN` - Cloudflare DNS API token
- `TRAEFIK_DASHBOARD_CREDENTIALS` - username:password for dashboard access
- `TUNNEL_TOKEN` - Cloudflare Tunnel token
- `TZ` - your timezone

3. Create required directories and files:
```bash
mkdir -p data
touch data/acme.json
chmod 600 data/acme.json
```

4. Start the stack:
```bash
docker compose up -d
```

## Important Notes

- Never commit sensitive files to git:
  - `.env`
  - `data/acme.json`
  - Any other files with tokens/passwords

- Required files structure:
```
traefik/
├── docker-compose.yml  # Contains all Traefik configuration
├── .env               # Your environment variables
├── env.example        # Template for .env
├── data/             # Not in git
│   └── acme.json    # SSL certificates
└── README.md
```

## Configuration
All Traefik configuration is done through command line arguments in `docker-compose.yml`. This includes:
- API and dashboard settings
- Entrypoints (HTTP/HTTPS)
- Docker provider configuration
- Let's Encrypt certificate resolver with Cloudflare DNS challenge

## Access
- Traefik Dashboard: `https://traefik.your-domain.com`
- Make sure your domain DNS is properly configured in Cloudflare 