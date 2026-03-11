---
name: obsidian-headless
description: Work with Obsidian vaults using official headless client (obsidian-headless). Supports vault sync, search, and file operations on Linux servers without GUI.
homepage: https://obsidian.md
metadata: {"clawdbot":{"emoji":"💎","requires":{"bins":["ob"]},"install":[{"id":"npm","kind":"npm","package":"obsidian-headless","bins":["ob"],"label":"Install obsidian-headless (npm)","global":true}]}}
---

# Obsidian Headless

Official CLI tool for Obsidian Sync without GUI. Perfect for Linux servers, automation, and AI agents.

## Installation

```bash
npm install -g obsidian-headless
```

The command is `ob` (not `obsidian-headless`).

## Quick Start

### 1. Login

```bash
# Login with email/password
ob login --email your@email.com --password 'yourpass'

# If 2FA enabled
ob login --email your@email.com --password 'yourpass' --mfa 123456

# Check login status
ob login
```

### 2. List Remote Vaults

```bash
ob sync-list-remote
```

Returns vault IDs and names like:
```
4ea5cadc738910a3842a2b0362c3f22e  "My Vault"  (Asia)
```

### 3. Setup Sync

```bash
ob sync-setup \
  --vault <vault-id-or-name> \
  --path /path/to/local/vault \
  --device-name "Server-Name" \
  --password 'encryption-password'
```

### 4. Sync

```bash
# One-time sync
ob sync --path /path/to/vault

# Continuous sync (daemon mode)
nohup ob sync --path /path/to/vault --continuous > ~/obsidian-sync.log 2>&1 &
```

## Common Operations

| Command | Description |
|---------|-------------|
| `ob sync-status --path <path>` | Check sync status |
| `ob sync-list-local` | List local vaults |
| `ob sync-unlink --path <path>` | Unlink vault from sync |
| `ob logout` | Logout |

## Troubleshooting

### "Another sync instance is already running"

```bash
pkill -9 -f "ob sync"
rm -rf /path/to/vault/.obsidian/.sync.lock
nohup ob sync --path /path/to/vault --continuous > ~/obsidian-sync.log 2>&1 &
```

## See Also

- Official docs: https://obsidian.md
- Obsidian Sync: https://obsidian.md/sync
