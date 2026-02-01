# OpenClaw VPS Ansible

Ansible playbook to prepare a fresh VPS for [OpenClaw](https://github.com/openclaw/openclaw) installation.

This playbook handles server hardening and basic setup — it does **not** install OpenClaw itself. Run this first, then follow the [OpenClaw installation guide](https://docs.openclaw.ai).

Based on [Kamal Ansible Manager](https://github.com/guillaumebriday/kamal-ansible-manager) by [Guillaume Briday](https://github.com/guillaumebriday).

## What it does

- **packages**: Installs essential packages (curl, git, htop, fail2ban, ufw, etc.), enables unattended upgrades, removes snap
- **firewall**: Configures UFW (deny incoming, allow outgoing, rate-limit SSH)
- **security**: Hardens SSH config (no password auth, no root password login), configures auto-reboot for security updates
- **docker** (optional): Installs Docker CE from official repo
- **reboot_if_needed**: Reboots if kernel updates require it

## Getting Started

1. Create an `inventory` file with your server IP:
```ini
[webservers]
your.server.ip.address
```

2. Install the requirements:
```bash
ansible-galaxy install -r requirements.yml
```

3. Run the playbook:
```bash
ansible-playbook playbook.yml
```

## Configuration

Variables in `playbook.yml`:

```yaml
vars:
  # Auto-reboot for security updates (default: false)
  security_autoupdate_reboot: "false"
  # Reboot time if enabled (24h format)
  security_autoupdate_reboot_time: "03:00"
```

To enable Docker, uncomment `- docker` in the roles list.

## After this playbook

Your VPS is ready. Now install OpenClaw following the [official docs](https://docs.openclaw.ai).

## License

MIT
