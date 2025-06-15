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
- `TUNNEL_TOKEN` - Cloudflare Tunnel token

3. Create secrets directory and token file:
```bash
mkdir -p secrets
nano secrets/cloudflare_api_token.txt
```
Add your Cloudflare API token to this file.

4. Set proper permissions:
```bash
chmod 600 secrets/cloudflare_api_token.txt
```

5. Start the stack:
```bash
docker compose up -d
```

## Important Notes

- Never commit sensitive files to git:
  - `.env`
  - `secrets/` directory
  - Any other files with tokens/passwords

- Required files structure:
```
traefik-proxy/
├── docker-compose.yml
├── .env                  # Your environment variables
├── env.example          # Template for .env
├── secrets/             # Not in git
│   └── cloudflare_api_token.txt
└── README.md
```

## Access
- Traefik Dashboard: `https://traefik.your-domain.com`
- Make sure your domain DNS is properly configured in Cloudflare 