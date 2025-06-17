# Docmost Setup

## Prerequisites

- Docker and Docker Compose installed
- Traefik network configured and running

## Setup

1. **Copy environment file:**
   ```bash
   cp env.example .env
   ```

2. **Edit environment variables:**
   ```bash
   nano .env
   ```
   
   Generate secure passwords:
   ```bash
   # For DOCMOST_SECRET (64 bytes base64)
   openssl rand -base64 64
   
   # For PostgreSQL password
   openssl rand -base64 24
   ```

3. **Create required directories:**
   ```bash
   mkdir -p postgres-data redis-data docmost-data
   ```

4. **Create required directories:**
   ```bash
   mkdir -p postgres-data redis-data docmost-data
   ```

5. **Start services:**
   ```bash
   docker-compose up -d
   ```

6. **Check container user ID and set permissions:**
   ```bash
   # Check what user ID the container runs as
   docker exec -it docmost id
   # Example output: uid=1000(node) gid=1000(node) groups=1000(node)
   
   # Set ownership based on the container's user ID (usually 1000)
   sudo chown -R 1000:1000 ./docmost-data ./postgres-data ./redis-data
   ```

6. **Check logs:**
   ```bash
   docker-compose logs -f
   ```

## Troubleshooting

- If you get permission errors, check container user IDs:
  ```bash
  docker exec -it docmost id
  docker exec -it docmost_db_1 id
  docker exec -it docmost_redis_1 id
  ```

- Verify traefik network exists:
  ```bash
  docker network ls | grep traefik
  ```

## Backup

Regular backup of data directories:
```bash
sudo tar -czf docmost-backup-$(date +%Y%m%d).tar.gz postgres-data redis-data docmost-data
``` 