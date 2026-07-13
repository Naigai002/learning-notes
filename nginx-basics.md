# Nginx Basics

Minimal notes for reverse proxy and static hosting.

## Install (Debian/Ubuntu)

```bash
sudo apt update
sudo apt install -y nginx
sudo systemctl enable --now nginx
```

## Static site

```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/example;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

## Reverse proxy

```nginx
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Useful commands

```bash
sudo nginx -t
sudo systemctl reload nginx
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

## Checklist

- Always run `nginx -t` before reload
- Prefer reverse proxy over exposing app ports publicly
- Keep configs under `/etc/nginx/sites-available` and enable with symlink
