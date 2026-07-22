# UFW Basics

Minimal firewall notes for Ubuntu hosts.

## Install and enable

```bash
sudo apt update
sudo apt install -y ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status verbose
```

## Common rules

```bash
# allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# allow from a trusted network
sudo ufw allow from 192.168.1.0/24 to any port 22

# delete a rule by number
sudo ufw status numbered
sudo ufw delete 3
```

## App profiles

```bash
sudo ufw app list
sudo ufw allow 'Nginx Full'
```

## Checklist

- Always allow SSH **before** enabling UFW on a remote host
- Prefer named app profiles when available
- Re-check `ufw status verbose` after changes
- Keep rules least-privilege: deny by default, open only needed ports
