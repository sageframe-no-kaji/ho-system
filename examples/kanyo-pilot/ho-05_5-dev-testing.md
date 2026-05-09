# Development Testing Strategy

## Volume-Mount Development Workflow

### Problem
Rebuilding and pulling 5GB Docker images for small code changes was extremely inefficient:
- Image rebuild: ~30 minutes
- Image pull on remote: ~10-15 minutes (depending on network)
- Total iteration time: 45+ minutes for a 2-line code fix

### Solution: Volume-Mounted Code
Instead of baking code into Docker images during development, we mount the code directory from the host into running containers. This allows instant code updates via git pull.

### Architecture

**Directory Structure on shingan.lan:**
```
/opt/services/
├── kanyo-admin/          # Docker Compose files, .env
├── kanyo-harvard/        # Stream 1 data, clips, logs
├── kanyo-nsw/           # Stream 2 data, clips, logs
└── kanyo-code/          # Git repository (mounted into containers)
```

**Volume Mount Configuration:**
```yaml
# docker-compose.nvidia.yml
volumes:
  - ${KANYO_CODE_ROOT:-/opt/services/kanyo-code}/src:/app/src:ro
```

The source code at `/opt/services/kanyo-code/src` is mounted read-only into `/app/src` inside both containers. This overrides the code baked into the image.

### Update Workflow

**Fast Development Iteration (11 seconds):**
```bash
# 1. Make changes locally
vim src/kanyo/detection/realtime_monitor.py

# 2. Commit and push
git add -A
git commit -m "Fix detection logic"
git push

# 3. Update remote and restart
./update-code.sh shingan.lan
```

**What `update-code.sh` does:**
```bash
# Pull latest code
ssh shingan.lan "cd /opt/services/kanyo-code && git pull"

# Restart containers (code remounted on restart)
ssh shingan.lan "cd /opt/services/kanyo-admin && sudo docker compose restart"
```

Total time: ~11 seconds vs ~45 minutes with image rebuilds.

### Deployment Script Enhancement

The `scripts/deploy-nvidia.sh` script now handles the git repository automatically:

```bash
# Step 1.5: Clone or update code repository
if [ -d "$CODE_DIR/.git" ]; then
    git pull
else
    git clone $GIT_REPO $CODE_DIR
fi
```

### When to Rebuild Images

**Volume mounts are for development only.** Rebuild images when:
- `requirements.txt` changes (new Python dependencies)
- System packages change (apt install)
- Creating production deployment
- Creating versioned releases

**Image rebuild workflow:**
```bash
# Local build
scripts/build-nvidia.sh

# Push to registry
docker push ghcr.io/sageframe-no-kaji/kanyo-contemplating-falcons-dev:nvidia

# Remote pull (only when needed)
ssh shingan.lan "cd /opt/services/kanyo-admin && sudo docker compose pull"
```

### Benefits

1. **Speed**: 11 seconds vs 45+ minutes per iteration
2. **Bandwidth**: No 5GB image transfers for code changes
3. **Iteration**: Can fix bugs and test rapidly
4. **Simplicity**: Standard git workflow, no Docker expertise needed
5. **Safety**: Read-only mount prevents container from modifying code

### Testing Strategy

**December 25, 2025 - Current Status:**

✅ **Both streams deployed and running on shingan.lan**
- Harvard stream: ROOSTING state (falcon detected, 0.84 confidence)
- NSW stream: ABSENT→ARRIVAL detection working with notifications

✅ **Volume mount strategy validated:**
- Code mounted from `/opt/services/kanyo-code/src` into both containers
- Git pull + container restart workflow tested (11 sec turnaround)
- Fixed 3 cascading bugs using rapid iteration

✅ **Production deployment ready:**
- `scripts/deploy-nvidia.sh` handles initial setup
- `scripts/deploy-production.sh` handles code updates
- `.env` configured with Telegram bot token
- Both containers running with GPU access

**Next Steps:**
1. Monitor for extended runtime stability
2. Verify state transitions (ARRIVAL, DEPARTURE, ROOSTING)
3. Confirm clip generation on events
4. Test Telegram notifications for both streams
5. Create versioned image when code stabilizes
