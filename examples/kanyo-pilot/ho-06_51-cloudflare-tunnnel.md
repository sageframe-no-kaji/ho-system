# Cloudflare Tunnel Setup for Kanyō (Docker)

## Prerequisites

- Docker installed on kanyo.lan
- Domain `sageframe.net` managed in Cloudflare
- Kanyō server running at `192.168.1.249:3000`

## Step 1: Create ZFS Dataset and Directory Structure

```bash
# Create ZFS dataset
sudo zfs create rpool/sage/kanyo/cloudflared

# Set mountpoint
sudo zfs set mountpoint=/opt/services/kanyo-cloudflared rpool/sage/kanyo/cloudflared

# Create config directory
sudo mkdir -p /opt/services/kanyo-cloudflared/config

# Set ownership (adjust user as needed)
sudo chown -R $USER:$USER /opt/services/kanyo-cloudflared

# Navigate to service directory
cd /opt/services/kanyo-cloudflared

# Special perms for cloudflare
sudo chown -R 65532:65532 /opt/services/kanyo-cloudflared/config
```

## Step 2: Authenticate with Cloudflare

```bash
# Run temporary container to authenticate
docker run -v /opt/services/kanyo-cloudflared/config:/home/nonroot/.cloudflared/ \
  cloudflare/cloudflared:latest tunnel login
```

This opens a browser - select your `sageframe.net` domain. Credentials are saved to `/opt/services/kanyo-cloudflared/config/cert.pem`

## Step 3: Create the Tunnel

```bash
# Create tunnel named "kanyo"
docker run -v /opt/services/kanyo-cloudflared/config:/home/nonroot/.cloudflared/ \
  cloudflare/cloudflared:latest tunnel create kanyo

# Note the tunnel ID from output (looks like: abc123-def456-ghi789)
```

ID: Created tunnel kanyo with id 
```
2535dda0-348a-4642-aeca-8d206980340d
```

Credentials file created at: `/opt/services/kanyo-cloudflared/config/<TUNNEL_ID>.json`

## Step 4: Configure DNS

```bash
# Point kanyo.sageframe.net to your tunnel
docker run -v /opt/services/kanyo-cloudflared/config:/home/nonroot/.cloudflared/ \
  cloudflare/cloudflared:latest tunnel route dns kanyo kanyo.sageframe.net
```

## Step 5: Create Config File

```bash
sudo nano /opt/services/kanyo-cloudflared/config/config.yml
```

Add this content:

```yaml
tunnel: <YOUR_TUNNEL_ID>
credentials-file: /home/nonroot/.cloudflared/<YOUR_TUNNEL_ID>.json

ingress:
  - hostname: kanyo.sageframe.net
    service: http://localhost:3000
  - service: http_status:404
```

**Replace:**

- `<YOUR_TUNNEL_ID>` with the ID from Step 3 (in both places)

**Note:** Using `localhost:3000` so the tunnel is independent of DHCP changes.

## Step 6: Create docker-compose.yml

```bash
nano /opt/services/kanyo-cloudflared/docker-compose.yml
```

```yaml
version: '3.8'

services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared-kanyo
    command: tunnel run
    volumes:
      - ./config:/home/nonroot/.cloudflared/
    network_mode: host
    restart: unless-stopped
```

**Note:** `network_mode: host` lets cloudflared access `localhost:3000` directly, avoiding DHCP dependency.

## Step 7: Start the Tunnel

```bash
cd /opt/services/kanyo-cloudflared
docker compose up -d
```

## Step 8: Verify

```bash
# Check logs
docker logs -f cloudflared-kanyo

# Visit your site
curl https://kanyo.sageframe.net
```

Visit `https://kanyo.sageframe.net` in browser - you should see Kanyo.

## Management Commands

```bash
# View status
docker ps | grep cloudflared

# View logs
docker logs -f cloudflared-kanyo

# Restart
docker compose restart

# Stop
docker compose down

# Start
docker compose up -d
```

## Troubleshooting

**Container exits immediately:**

```bash
docker logs cloudflared-kanyo
# Check for credential or config errors
```

**Can't reach Kanyō service:**

- Verify Kanyō is running: `curl http://localhost:3000`
- Check that Kanyō is bound to `0.0.0.0:3000` (not just `127.0.0.1:3000`)
- Verify `network_mode: host` is set in docker-compose.yml

**DNS not resolving:**

- Wait 1-2 minutes for DNS propagation
- Check Cloudflare dashboard DNS records

**Still issues?**

- Check cloudflared logs: `docker logs -f cloudflared-kanyo`
- Verify tunnel is connected in Cloudflare dashboard