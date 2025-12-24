# n8n - Workflow Automation

n8n is a free and open-source workflow automation tool.

## Quick Start

### 1. Navigate to the n8n directory

```bash
cd docker/n8n
```

### 2. (Optional) Create environment file

Create a `.env` file to customize your setup:

```bash
cp .env.example .env
```

Edit the values as needed for your homelab.

### 3. Start n8n

```bash
docker compose up -d
```

### 4. Access n8n

Open your browser and go to:

```
http://<your-server-ip>:5678
```

On first access, you'll be prompted to create an owner account.

## Configuration

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `N8N_HOST` | localhost | Hostname for n8n |
| `N8N_PROTOCOL` | http | Protocol (http/https) |
| `WEBHOOK_URL` | http://localhost:5678/ | URL for webhooks |
| `GENERIC_TIMEZONE` | Europe/Berlin | Timezone for workflows |
| `TZ` | Europe/Berlin | Container timezone |

### Data Persistence

All n8n data (workflows, credentials, settings) is stored in the `n8n_data` Docker volume.

## Management Commands

```bash
# Start
docker compose up -d

# Stop
docker compose down

# View logs
docker compose logs -f n8n

# Update to latest version
docker compose pull && docker compose up -d

# Backup data volume
docker run --rm -v n8n_n8n_data:/data -v $(pwd):/backup alpine tar czf /backup/n8n-backup.tar.gz -C /data .
```

## Resource Usage

n8n is lightweight:
- ~200-300 MB RAM at idle
- Increases based on workflow complexity and concurrent executions

## Useful Links

- [n8n Documentation](https://docs.n8n.io/)
- [Workflow Templates](https://n8n.io/workflows/)
- [Community Forum](https://community.n8n.io/)
