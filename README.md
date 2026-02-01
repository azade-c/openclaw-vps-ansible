# OpenClaw VPS Ansible

Ansible playbook to prepare a fresh VPS for [OpenClaw](https://github.com/openclaw/openclaw) installation.

This playbook handles server hardening and basic setup — it does **not** install OpenClaw itself. Run this first, then follow the [OpenClaw installation guide](https://docs.openclaw.ai).

Based on [Kamal Ansible Manager](https://github.com/guillaumebriday/kamal-ansible-manager) by [Guillaume Briday](https://github.com/guillaumebriday).

## Compatibility

**Operating Systems:**
- Ubuntu (20.04+)
- Debian (11+)

**Architectures:**
- AMD64 / x86_64
- ARM64 / aarch64

## What it does

- **packages**: Installs essential packages (curl, git, htop, fail2ban, ufw, etc.), enables unattended upgrades, removes snap
- **firewall**: Configures UFW (deny incoming, allow outgoing, rate-limit SSH)
- **security**: Hardens SSH config (no password auth, no root password login), configures auto-reboot for security updates
- **docker** (optional): Installs Docker CE from official repo
- **reboot_if_needed**: Reboots if kernel updates require it

### About Docker

Docker is **disabled by default**. You'll need to enable it if you plan to use OpenClaw's sandboxed agents feature, which runs code in isolated containers.

To enable Docker, uncomment `- docker` in `playbook.yml`:

```yaml
roles:
  - packages
  - docker  # uncomment this line
  - firewall
  - security
  - reboot_if_needed
```

## Prerequisites

Install Ansible on your local machine (the one you'll run the playbook from).

**macOS (Homebrew):**
```bash
brew install ansible
```

**Ubuntu/Debian:**
```bash
sudo apt update && sudo apt install ansible
```

**pip (any platform):**
```bash
pip install ansible
```

## Getting Started

1. Clone this repository:
```bash
git clone https://github.com/azade-c/openclaw-vps-ansible.git
cd openclaw-vps-ansible
```

2. Create an `inventory` file with your server IP:
```ini
[webservers]
your.server.ip.address
```

3. Install Ansible Galaxy requirements:
```bash
ansible-galaxy install -r requirements.yml
```

4. Run the playbook:
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

## After this playbook

Your VPS is ready for OpenClaw. Follow the [official installation guide](https://docs.openclaw.ai) to complete the setup.

## License

MIT
