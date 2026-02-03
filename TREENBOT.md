# TreenBot Setup Reference

## Two Instances Configuration

```
┌─────────────────────────────────────────────────────────────┐
│                 ~/.clawdbot/ (SHARED CONFIG)                │
│  - clawdbot.json (config file)                              │
│  - credentials/ (API keys, OAuth tokens)                    │
│  - sessions/ (conversation history - SHARED!)               │
│  - agents/, telegram/, devices/, etc.                       │
└─────────────────────────────────────────────────────────────┘
         ▲                                    ▲
         │                                    │
    ┌────┴─────┐                      ┌──────┴──────┐
    │   NPM     │                      │   SOURCE    │
    │ VERSION   │                      │   VERSION   │
    │ (Production)                      │  (Dev/Play) │
    └───────────┘                      └─────────────┘
    Port: 4430                         Port: 18789
    Auto-start                         Manual only
    Not updatable                      You edit code
```

---

## Instance 1: NPM Version (Production/Background)

**Purpose:** Runs always in background, starts with computer

**Port:** 4430

**To install with custom port:**
```bash
# Set environment variable for port
set CLAWDBOT_GATEWAY_PORT=4430

# Install from npm (if not already installed)
npm install -g moltbot

# OR if already installed, just set the port in config:
clawdbot config set gateway.port 4430
```

**To run manually (test):**
```bash
# Set port and run
set CLAWDBOT_GATEWAY_PORT=4430
moltbot gateway run --bind loopback --port 4430 --force
```

**To start gateway in background:**
```bash
# Windows - run in background
start /B moltbot gateway run --bind loopback --port 4430 --force > C:\Users\Treentity\clawdbot-gateway.log 2>&1
```

**Control UI at:** `http://localhost:4430` (when gateway is running)

---

## Instance 2: Source Version (Your Dev/Playground)

**Branch:** `treenbot`

**Port:** 18789 (default)

**Location:** `C:\Users\Treentity\Documents\Projects\clawdbot`

**To build after code changes:**
```bash
cd C:\Users\Treentity\Documents\Projects\clawdbot
pnpm build
```

**To run gateway:**
```bash
cd C:\Users\Treentity\Documents\Projects\clawdbot
clawdbot gateway run --bind loopback --port 18789
```

**Control UI at:** `http://localhost:18789` (when gateway is running)

---

## OAuth / Token Verification

**Access control UI:** `http://localhost:4430` (NPM) or `http://localhost:18789` (Source)

**When prompted for token:**
1. Check your config: `cat ~/.clawdbot/clawdbot.json | findstr token`
2. Or generate new one: `clawdbot config set gateway.auth.token <new-token>`

**Current token:** `179a42ef8940bc0db965dcc894c03ccfca47fef52d4ec63d`

---

## Important: ONLY ONE GATEWAY RUNNING AT A TIME!

**Before switching instances:**

1. **Stop running gateway:**
   ```bash
   # Find and kill the process
   taskkill /F /IM node.exe /FI "WINDOWTITLE eq moltbot*"
   # OR
   clawdbot gateway stop
   ```

2. **Verify it's stopped:**
   ```bash
   netstat -ano | findstr :4430
   netstat -ano | findstr :18789
   ```

3. **Start the other instance**

---

## Common Commands for Both

```bash
# Check status
clawdbot channels status

# Send a test message
clawdbot message send --to telegram --text "test"

# View logs
clawdbot logs follow

# Configure
clawdbot config set <key> <value>

# View config
clawdbot config get
```

---

## Branch Strategy (Updates)

```
main          → Tracks upstream (moltbot/moltbot) for clean updates
treenbot      → Your personal changes and experiments
```

**To get updates from upstream:**
```bash
# 1. Update main branch
git checkout main
git fetch upstream
git merge upstream/main

# 2. Merge updates into your treenbot branch
git checkout treenbot
git merge main
# Resolve any conflicts (keep your changes)
```

**To rebuild after updates:**
```bash
cd C:\Users\Treentity\Documents\Projects\clawdbot
pnpm build
```

---

## Quick Reference: Which Instance Am I Using?

| Command | Instance | Port | Notes |
|---------|----------|------|-------|
| `moltbot gateway run` | NPM | 4430 | Production |
| `clawdbot gateway run` | Source | 18789 | Dev/Playground |

**Check what's listening on a port:**
```bash
netstat -ano | findstr :4430
netstat -ano | findstr :18789
```

---

## Startup Configuration (Auto-start with Windows)

To make NPM version start automatically with Windows, create a scheduled task or use startup folder:

**Option 1: Startup Folder**
```bash
# Create shortcut in shell:startup folder
# Target: moltbot gateway run --bind loopback --port 4430 --force
```

**Option 2: Windows Service (advanced)**
- Use NSSM (Non-Sucking Service Manager)
- Install as Windows service

---

## Troubleshooting

**Port already in use:**
```bash
# Find what's using the port
netstat -ano | findstr :4430

# Kill the process
taskkill /F /PID <PID from above>
```

**Check which instance you're running:**
```bash
# NPM version
moltbot --version

# Source version
clawdbot --version
```

**Both use same config, so:**
- Same Telegram bot token
- Same API credentials
- Same conversation history
- NO need to re-authenticate!

---

## File Locations

| Item | Location |
|------|----------|
| Source code | `C:\Users\Treentity\Documents\Projects\clawdbot` |
| Config | `C:\Users\Treentity\.clawdbot\clawdbot.json` |
| Sessions | `C:\Users\Treentity\.clawdbot\sessions\` |
| Credentials | `C:\Users\Treentity\.clawdbot\credentials\` |
| Logs | `C:\Users\Treentity\.clawdbot\logs\` |
