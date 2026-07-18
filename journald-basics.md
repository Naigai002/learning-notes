# Journald Basics

Reading and filtering systemd logs.

## Common queries

```bash
# service logs
journalctl -u nginx.service -n 100 --no-pager

# follow live
journalctl -u nginx.service -f

# since boot / time window
journalctl -b
journalctl --since "1 hour ago" --until "now"
```

## Priority and fields

```bash
# 0=emerg ... 3=err ... 6=info
journalctl -p err -b

# by executable / PID
journalctl /usr/sbin/sshd
journalctl _PID=1234
```

## Export and size

```bash
journalctl -u myapp -o json-pretty > app.json
journalctl --disk-usage
sudo journalctl --vacuum-size=200M
```

## Checklist

- Prefer `journalctl -u <unit>` over raw log files when systemd manages the service
- Use `--no-pager` in scripts
- Vacuum old logs carefully on low-disk hosts
