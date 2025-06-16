# Home Lab Management Script

Simple bash script for managing Docker Compose services in your home lab.

## Setup

1. Make the script executable:
```bash
chmod +x mng
```

## Usage

The script supports two modes of operation:

### Single Service
```bash
./mng <service> <command>
```

Example:
```bash
./mng gitea start
./mng gitea stop
./mng gitea restart
```

### All Services
```bash
./mng all <command>
```

Example:
```bash
./mng all start
./mng all stop
./mng all restart
```

## Commands

- `start` - starts the service(s) using `docker-compose up -d`
- `stop` - stops the service(s) using `docker-compose down`
- `restart` - stops and then starts the service(s)

## Requirements

- Docker
- Docker Compose
- Bash shell

## Notes

- The script must be run from the directory where it's located
- Each service should be in its own directory with a `docker-compose.yml` file
- If a service directory doesn't exist or doesn't contain a `docker-compose.yml`, the script will show an error 