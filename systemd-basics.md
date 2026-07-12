# Systemd Basics

## Unit Types

| Type | Purpose |
|------|---------|
| `.service` | Daemon / application |
| `.timer` | Cron-like scheduler |
| `.socket` | Socket activation |
| `.mount` | Mount points |

## Common Commands

```bash
systemctl status nginx
journalctl -u nginx -f
systemctl enable --now nginx
```