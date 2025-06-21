# Home Lab Management Script

Simple bash script for managing Docker Compose services in your home lab.

## Setup

1. Make the script executable:
```bash
chmod +x mng
chmod +x bkp
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

## Backup Script

The `bkp` script provides automated backup and restore functionality for Docker volumes.

### Backup Individual Services
```bash
./bkp <service_name>
```

Example:
```bash
./bkp docmost      # backup only docmost volumes
./bkp gitea        # backup only gitea volumes
./bkp planka       # backup only planka volumes
./bkp vaultwarden  # backup only vaultwarden volumes
```

### Backup All Services
```bash
./bkp all
```

Creates compressed archives of all Docker volumes in the `./backups/` directory with timestamped filenames. Automatically removes old backups, keeping only the 3 most recent for each volume.

### Restore Individual Services
```bash
./bkp restore <service_name>
```

Example:
```bash
./bkp restore docmost      # restore only docmost volumes
./bkp restore gitea        # restore only gitea volumes
./bkp restore planka       # restore only planka volumes
./bkp restore vaultwarden  # restore only vaultwarden volumes
```

### Restore All Services
```bash
./bkp restore all
```

Restores all volumes from their latest available backup. Stops all containers before restoration and provides instructions to restart them.

### Available Services

- `docmost` - Docmost volumes (docmost-data, postgres-data, redis-data)
- `gitea` - Gitea volumes (gitea-data, postgres-data)
- `planka` - Planka volumes (user-avatars, favicons, background-images, attachments, postgres-data)
- `vaultwarden` - Vaultwarden volumes (vaultwarden-data)

### Automated Backup Setup

To set up daily automated backups at 3:00 AM using systemd:

1. Create service file:
```bash
sudo nano /etc/systemd/system/home-lab-backup.service
```

Content:
```ini
[Unit]
Description=Home Lab Docker Volumes Backup
After=docker.service
Requires=docker.service

[Service]
Type=oneshot
User=YOUR_USERNAME
Group=YOUR_USERNAME
WorkingDirectory=/path/to/your/home-lab
ExecStart=/path/to/your/home-lab/bkp all
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

2. Create timer file:
```bash
sudo nano /etc/systemd/system/home-lab-backup.timer
```

Content:
```ini
[Unit]
Description=Run Home Lab Backup daily at 3:00 AM
Requires=home-lab-backup.service

[Timer]
OnCalendar=*-*-* 03:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

3. Enable and start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable home-lab-backup.timer
sudo systemctl start home-lab-backup.timer
```

4. Useful management commands:
```bash
# Check next run time
sudo systemctl list-timers home-lab-backup.timer

# View backup logs
sudo journalctl -u home-lab-backup.service -f

# Manual backup
sudo systemctl start home-lab-backup.service

# Stop automatic backups
sudo systemctl stop home-lab-backup.timer
sudo systemctl disable home-lab-backup.timer
```

## Requirements

- Docker
- Docker Compose
- Bash shell

## Notes

- The script must be run from the directory where it's located
- Each service should be in its own directory with a `docker-compose.yml` file
- If a service directory doesn't exist or doesn't contain a `docker-compose.yml`, the script will show an error
- Backup files are stored in `./backups/` directory and are excluded from git via `.gitignore`
- Individual service backup/restore stops only the relevant containers
- All service backup/restore stops all containers before operation 