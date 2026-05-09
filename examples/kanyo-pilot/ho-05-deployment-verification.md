# Ho-05: Deployment Verification & Monitoring

**Duration:** 2-3 hours
**Goal:** Deploy Kanyō to production environment (Proxmox/DO), verify it works, establish monitoring
**Deliverable:** System running reliably with monitoring in place, ready for frontend development

---

## Why This Ho Matters

You have working code on Mac. Now you need to:
1. Deploy to production environment (not the cursed HP ProDesk)
2. Verify it actually works in production
3. Set up monitoring so you know if it breaks
4. Establish baseline performance metrics

**After this Ho:** You can confidently build frontends knowing the backend is solid.

---

## Part 1: Choose Your Deployment Target (10 minutes)

### Option A: Proxmox LXC (Recommended)
**Pros:**
- Free
- Hardware you control
- GPU available later
- Fast local network

**Setup:**
1. Create Debian 13 LXC (unprivileged, nesting + keyctl enabled)
2. 2 vCPU, 4GB RAM, 20GB disk
3. Install Docker: `curl -fsSL https://get.docker.com | sh`

### Option B: DigitalOcean
**Pros:**
- Reliable datacenter hardware
- No cursed HP ProDesk issues
- Simple setup

**Cost:** $6-12/month (cheap for peace of mind)

**Setup:**
1. Create Debian 13 droplet ($6 or $12/month)
2. SSH in
3. Install Docker: `curl -fsSL https://get.docker.com | sh`

---

## Part 2: Deploy the Stack (30 minutes)

### Step 2.1: Prepare Deployment Environment

**SSH into your deployment target:**
```bash
# Proxmox LXC
ssh root@<lxc-ip>

# Or DigitalOcean
ssh root@<droplet-ip>
```

**Create directory structure:**
```bash
mkdir -p /opt/kanyo/{configs,clips,logs}
cd /opt/kanyo
```

### Step 2.2: Create docker-compose.yml

**For single stream (start simple):**
```yaml
version: '3.8'

services:
  nsw:
    image: ghcr.io/sageframe-no-kaji/kanyo-contemplating-falcons-dev:cpu
    container_name: kanyo-nsw
    volumes:
      - ./configs/nsw.yaml:/app/config.yaml:ro
      - ./clips:/app/clips
      - ./logs:/app/logs
    environment:
      - PYTHONUNBUFFERED=1
      - TELEGRAM_BOT_TOKEN=${TELEGRAM_BOT_TOKEN}
      - TELEGRAM_CHANNEL=${TELEGRAM_CHANNEL}
    restart: unless-stopped
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

**Save as `/opt/kanyo/docker-compose.yml`**

### Step 2.3: Create Stream Config

**Copy your working config from Mac:**
```bash
# On Mac
scp ~/path/to/test_config_nsw.yaml root@<target>:/opt/kanyo/configs/nsw.yaml
```

**Or create directly:**
```yaml
# /opt/kanyo/configs/nsw.yaml
video_source: "https://www.youtube.com/watch?v=yv2RtoIMNzA"

# Detection
detection_confidence: 0.5
frame_interval: 3
model_path: "/root/.u8/yolov8n.pt"
detect_any_animal: true
exit_timeout: 60
visit_merge_timeout: 60
animal_classes: [14, 15, 16, 17, 18, 19, 20, 21, 22, 23]

# Clips
clips_dir: "/app/clips"
log_file: "/app/logs/kanyo.log"

clip_entrance_before: 5
clip_entrance_after: 15
clip_exit_before: 15
clip_exit_after: 5
clip_merge_threshold: 180

thumbnail_entrance_offset: 5
thumbnail_exit_offset: -10

clip_compress: true
clip_crf: 23
clip_fps: 30
clip_hardware_encoding: false  # CPU-only for now

# Live stream
live_use_ffmpeg_tee: true
live_proxy_url: "udp://127.0.0.1:12345"
buffer_dir: "/tmp/kanyo-buffer"
continuous_chunk_minutes: 10

# Notifications
telegram_enabled: true
telegram_channel: "@your_channel_here"
ntfy_admin_enabled: false
notification_cooldown_minutes: 5

# Logging
log_level: "INFO"
```

### Step 2.4: Set Environment Variables

**Create `.env` file:**
```bash
# /opt/kanyo/.env
TELEGRAM_BOT_TOKEN=your_bot_token_here
TELEGRAM_CHANNEL=@your_channel_or_chat_id
```

**Get Telegram credentials:**
1. Create bot: Talk to @BotFather on Telegram
2. Get channel: Create public channel or get chat ID
3. Add bot to channel as admin

### Step 2.5: Pull Image and Start

```bash
cd /opt/kanyo

# Pull the image (this is the test!)
docker compose pull

# Start the container
docker compose up -d

# Watch logs
docker compose logs -f
```

**Expected output:**
```
kanyo-nsw | Loading YOLO model...
kanyo-nsw | Model loaded successfully
kanyo-nsw | Starting Real-Time Falcon Monitoring
kanyo-nsw | Stream: https://www.youtube.com/watch?v=...
kanyo-nsw | Connected to stream
```

---

## Part 3: Verification Tests (45 minutes)

### Test 1: Stream Connection (5 minutes)

**Watch logs for connection:**
```bash
docker compose logs -f nsw | grep -i "connect"
```

**Success criteria:**
- ✅ "Resolving stream URL"
- ✅ "Connected to stream"
- ✅ No "Connection refused" or "timeout" errors

### Test 2: Detection Running (10 minutes)

**Watch for detection activity:**
```bash
docker compose logs -f nsw | grep -i "falcon\|detection"
```

**Success criteria:**
- ✅ Frames being processed
- ✅ If falcon present: "FALCON ENTERED" messages
- ✅ No crash/restart loops

**Force a detection (optional):**
```bash
# Check if there's activity in the stream right now
# You should see detections within 10-15 minutes if falcon is present
```

### Test 3: Clip Creation (15 minutes)

**Wait for falcon activity, then check:**
```bash
# Check clips directory
ls -lh /opt/kanyo/clips/$(date +%Y-%m-%d)/

# Should see:
# - falcon_HHMMSS_arrival.mp4
# - falcon_HHMMSS_arrival.jpg
# - falcon_HHMMSS_departure.mp4
# - events_YYYY-MM-DD.json
```

**Verify clip is valid:**
```bash
# Check file size (should be several MB)
ls -lh /opt/kanyo/clips/$(date +%Y-%m-%d)/*.mp4

# Download one clip to Mac and play it
scp root@<target>:/opt/kanyo/clips/*/falcon_*.mp4 ~/Downloads/
```

### Test 4: Notifications (5 minutes)

**Check your Telegram:**
- ✅ Received arrival notification with image
- ✅ Received departure notification with duration

**If not working:**
```bash
# Check logs for notification errors
docker compose logs nsw | grep -i "telegram\|notification"

# Verify env vars loaded
docker compose exec nsw env | grep TELEGRAM
```

### Test 5: Resource Usage (10 minutes)

**Check container stats:**
```bash
docker stats kanyo-nsw
```

**Expected:**
- CPU: 60-80% of 1 core (acceptable)
- Memory: 500MB-1GB (fine)
- Network: Continuous traffic (streaming)

**If CPU is 100% constantly:**
- Increase `frame_interval` from 3 to 5
- Or resize to 2 vCPU

**Check host system:**
```bash
# Proxmox
top
htop

# Should have plenty of headroom
```

---

## Part 4: Monitoring Setup (30 minutes)

### Step 4.1: Create Monitoring Script

**Create `/opt/kanyo/monitor.sh`:**
```bash
#!/bin/bash
# Simple monitoring script for Kanyō

echo "=== Kanyō System Status ==="
echo "Time: $(date)"
echo

echo "=== Container Status ==="
docker compose ps
echo

echo "=== Resource Usage ==="
docker stats --no-stream kanyo-nsw
echo

echo "=== Recent Clips ==="
find /opt/kanyo/clips -name "*.mp4" -mtime -1 -exec ls -lh {} \;
echo

echo "=== Recent Errors ==="
docker compose logs --tail=50 nsw | grep -i "error\|warning\|fail" || echo "No recent errors"
echo

echo "=== Disk Space ==="
df -h /opt/kanyo
echo

echo "=== Last 5 Log Lines ==="
docker compose logs --tail=5 nsw
```

**Make executable:**
```bash
chmod +x /opt/kanyo/monitor.sh
```

**Run it:**
```bash
/opt/kanyo/monitor.sh
```

### Step 4.2: Set Up Log Rotation

**Already handled by docker-compose logging config**, but verify:
```bash
# Check Docker log size
ls -lh /var/lib/docker/containers/*/kanyo-nsw*.log
```

**Should be < 30MB (10MB × 3 files)**

### Step 4.3: Create Health Check

**Add to docker-compose.yml:**
```yaml
services:
  nsw:
    # ... existing config ...
    healthcheck:
      test: ["CMD", "pgrep", "-f", "realtime_monitor"]
      interval: 60s
      timeout: 10s
      retries: 3
      start_period: 120s
```

**Reload:**
```bash
docker compose up -d
```

**Check health:**
```bash
docker compose ps
# Should show "healthy" status after 2 minutes
```

### Step 4.4: Set Up Alerts (Optional but Recommended)

**Create ntfy admin topic for errors:**
```yaml
# Add to nsw.yaml
ntfy_admin_enabled: true
# Then set in .env:
NTFY_ADMIN_TOPIC=kanyo_admin_alerts
```

**Subscribe to admin topic on your phone:**
- Install ntfy app
- Subscribe to `kanyo_admin_alerts`
- Get notified if system has issues

---

## Part 5: Stress Test (30 minutes)

### Test 5.1: Continuous Operation

**Let it run for 30-60 minutes:**
```bash
# Start monitoring in one terminal
watch -n 30 /opt/kanyo/monitor.sh

# Watch logs in another
docker compose logs -f
```

**What to watch for:**
- ❌ Memory leak (memory climbing continuously)
- ❌ CPU spiking to 100%
- ❌ Container restarting
- ❌ Stream disconnections
- ✅ Stable resource usage
- ✅ Continuous frame processing

### Test 5.2: Restart Recovery

**Test container restart:**
```bash
# Restart container
docker compose restart nsw

# Watch it recover
docker compose logs -f nsw
```

**Success criteria:**
- ✅ Reconnects to stream within 30 seconds
- ✅ Resumes detection
- ✅ No data loss (clips preserved)

### Test 5.3: System Reboot

**Test full system restart:**
```bash
# Reboot the host
sudo reboot

# Wait 2 minutes, SSH back in
ssh root@<target>

# Check if container auto-started
docker compose ps

# Should show "Up" with restart count
```

**Success criteria:**
- ✅ Container started automatically
- ✅ Stream reconnected
- ✅ Detection resumed

---

## Part 6: Performance Baseline (15 minutes)

### Document Current Performance

**Create `/opt/kanyo/BASELINE.md`:**
```markdown
# Kanyō Performance Baseline

**Date:** 2025-12-23
**Environment:** Proxmox LXC / DigitalOcean $6 droplet
**Image:** ghcr.io/.../kanyo:cpu

## Resource Usage (Typical)
- CPU: 60-80% of 1 core
- Memory: 700MB
- Network: 2-5 Mbps (streaming)
- Disk I/O: Low (only during clip creation)

## Detection Performance
- Frame rate: ~10 fps (processing every 3rd frame)
- Detection latency: 150-200ms per frame
- Stream reconnection: ~10 seconds

## Clip Performance
- Arrival clip: Created 15s after entrance
- Departure clip: Created after exit timeout (60s)
- Clip size: 5-15MB for 30-second clip (CRF 23)
- Encoding speed: ~30 seconds to encode 30-second clip (CPU)

## Notification Performance
- Delivery latency: 2-5 seconds
- Success rate: 100% (so far)
- Cooldown: 5 minutes between arrivals

## Issues Encountered
- None yet

## Notes
- Running CPU-only baseline
- Hardware encoding disabled (no GPU)
- Single stream (NSW)
```

**Save this for comparison when you add features.**

---

## Ho-05 Completion Checklist

**Deployment:**
- [ ] Environment chosen (Proxmox or DO)
- [ ] Docker installed
- [ ] Image pulled successfully
- [ ] Container running

**Verification:**
- [ ] Stream connects
- [ ] Detection working
- [ ] Clips created (if falcon present)
- [ ] Notifications received
- [ ] Resource usage acceptable

**Monitoring:**
- [ ] `monitor.sh` script created
- [ ] Health check configured
- [ ] Admin alerts set up (optional)
- [ ] Baseline documented

**Stress Tests:**
- [ ] Ran for 30-60 minutes without issues
- [ ] Container restart recovery tested
- [ ] System reboot recovery tested

---

## Troubleshooting

### Container Won't Start

**Check logs:**
```bash
docker compose logs nsw
```

**Common issues:**
- Config file syntax error → validate YAML
- Missing env vars → check `.env` file
- Permission errors → check directory ownership

### Image Pull Fails

**Check network:**
```bash
# Can you reach GHCR?
curl -I https://ghcr.io

# Try pulling directly
docker pull ghcr.io/sageframe-no-kaji/kanyo-contemplating-falcons-dev:cpu
```

**If still fails:**
- Check if image is public
- Try from Mac (verify image exists)
- Check Docker daemon: `systemctl status docker`

### High CPU Usage

**Solutions:**
- Increase `frame_interval` (3 → 5)
- Reduce detection confidence (0.5 → 0.6)
- Add more vCPU (1 → 2)

### Memory Issues

**Check swap:**
```bash
free -h
```

**If swapping:**
- Increase container RAM (1GB → 2GB)
- Or reduce `frame_interval` to process fewer frames

### No Clips Created

**Check:**
```bash
# Is tee mode enabled?
grep live_use_ffmpeg_tee configs/nsw.yaml

# Is buffer directory writable?
docker compose exec nsw ls -la /tmp/kanyo-buffer/

# Are segments being created?
docker compose exec nsw ls -la /tmp/kanyo-buffer/*.ts
```

---

## What's Next?

**Ho-06: Admin Web Interface**
- Build Flask/FastAPI admin panel
- View logs in browser
- Manage multiple streams
- View clips gallery (local)

**After that:**
**Ho-07: Static Site Generation**
- Generate public gallery from events
- Deploy to Cloudflare Pages
- Community viewing

---

## Success Criteria

**You're ready for Ho-06 when:**
- ✅ Container runs for 1+ hour without restart
- ✅ Clips are created successfully
- ✅ Notifications work
- ✅ Resource usage is stable
- ✅ System survives reboot

**You've proven:** The deployment is solid and ready for frontend work.

---

## Actual Deployment Log

**Completed:** 2025-12-24
**Deployment Target:** Proxmox VM `shingan` (192.168.1.22) with NVIDIA RTX 3050
**Time Spent:** ~4 hours
**Issues Encountered:**
- Initial permission errors on log directories (resolved with chmod)
- Had to shut down duplicate deployment on 192.168.1.252 to avoid double notifications
**Ready for Ho-06:** Yes

### Final Deployment Configuration

**Environment:** Proxmox VM `shingan` running Ubuntu 24.04.3 LTS
**Hardware:** NVIDIA GeForce RTX 3050 (8GB VRAM) with CUDA 12.2 passthrough
**Docker Image:** `ghcr.io/sageframe-no-kaji/kanyo-contemplating-falcons-dev:nvidia`
**Location:** `/opt/services/kanyo-gpu/`

**Containers Running:**
- `kanyo-nsw-gpu` - NSW Peregrine Falcon Cam
- `kanyo-harvard-gpu` - Harvard Peregrine Falcon Cam

Both containers using GPU acceleration for YOLO inference.

### Verification Results

**✅ Stream Connection:**
- Both streams connected successfully via ffmpeg tee mode
- Using software encoder (libx264) for clips
- Continuous stream capture working

**✅ GPU Access:**
```
NVIDIA GeForce RTX 3050 OEM
Driver Version: 535.274.02
CUDA Version: 12.2
GPU Memory: 325MiB / 8192MiB allocated
```

**✅ Detection Performance:**
- Model loaded successfully (yolov8n.pt)
- Detection confidence: 0.3
- Frame interval: 1 (processing every frame)
- Detection running smoothly

**✅ Resource Usage:**
- GPU: 0% idle (will increase with detections)
- Container memory: ~325MB GPU + system RAM
- CPU: Minimal (offloaded to GPU)

**✅ Auto-restart:**
- Configured with `restart: unless-stopped`
- Tested container restart - recovered successfully

**✅ Notifications:**
- Telegram bot configured: `@kanyo_nsw_falcon_cam` and `@kanyo_harvard_falcon_cam`
- Cooldown: 5 minutes
- Both channels active

### Configuration Files

**docker-compose.yml:**
- Using nvidia runtime with GPU reservations
- Separate volumes for harvard and nsw data/configs
- 2GB shared memory allocated per container
- JSON logging with 10MB max file size, 3 file rotation

**Config files:**
- `/opt/services/kanyo-gpu/data/nsw/config.yaml`
- `/opt/services/kanyo-gpu/data/harvard/config.yaml`

### Commands Reference

**View logs:**
```bash
cd /opt/services/kanyo-gpu
docker compose logs -f nsw-gpu
docker compose logs -f harvard-gpu
```

**Restart containers:**
```bash
docker compose restart
```

**Pull updated image:**
```bash
docker compose pull
docker compose up -d
```

**Check GPU status:**
```bash
docker exec kanyo-nsw-gpu nvidia-smi
```

**Monitor container status:**
```bash
docker compose ps
docker stats kanyo-nsw-gpu kanyo-harvard-gpu
```

### Next Steps

- [x] GPU deployment complete
- [x] Both streams monitoring
- [x] Notifications configured
- [ ] Wait for clip generation to verify full pipeline
- [ ] Build admin interface (Ho-06)
- [ ] Static site generation (Ho-07)
