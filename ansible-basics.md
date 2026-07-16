# Ansible Basics

Practical notes for inventory, ad-hoc commands, and simple playbooks.

## Inventory

`hosts.ini`:

```ini
[web]
web1.example.com
web2.example.com

[db]
db1.example.com

[all:vars]
ansible_user=ubuntu
```

Or YAML inventory:

```yaml
all:
  children:
    web:
      hosts:
        web1.example.com:
        web2.example.com:
  vars:
    ansible_user: ubuntu
```

## Ad-hoc commands

```bash
ansible all -i hosts.ini -m ping
ansible web -i hosts.ini -m shell -a "uptime"
ansible web -i hosts.ini -m apt -a "name=nginx state=present" --become
ansible web -i hosts.ini -m copy -a "src=./app.conf dest=/etc/app/app.conf" --become
```

## Playbook structure

```yaml
- name: Configure web servers
  hosts: web
  become: true
  vars:
    app_port: 8080
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: true

    - name: Deploy config
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/app
      notify: Reload nginx

    - name: Ensure service is running
      service:
        name: nginx
        state: started
        enabled: true

  handlers:
    - name: Reload nginx
      service:
        name: nginx
        state: reloaded
```

## Common modules

| Module | Use |
|---|---|
| `copy` | copy local file to remote |
| `template` | render Jinja2 template |
| `service` | start/stop/enable services |
| `apt` / `yum` | package management |
| `file` | create dirs / set permissions |
| `user` | manage users |

## Variables & facts

```bash
# show gathered facts for one host
ansible web1.example.com -i hosts.ini -m setup
```

In playbooks:

```yaml
- debug:
    msg: "os={{ ansible_distribution }} host={{ inventory_hostname }}"
```

## Tips

- Start with ad-hoc checks before writing playbooks
- Keep inventories and secrets separate
- Prefer idempotent modules over raw shell when possible
- Use handlers for restarts/reloads after config changes
