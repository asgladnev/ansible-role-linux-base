# ansible-role-linux-base

**Base Linux hardening & configuration role** for Debian systems.
Configures:

- System hardening (sysctl)
- SSH daemon (security & authentication)
- NTP synchronization (chrony)
- Automatic security updates (unattended-upgrades)
- Bash shell settings
- Base system utilities and packages

---

## Requirements

- Ansible >= 2.15
- Python packages (from `requirements.txt`)
- `ansible.posix` collection >= 1.5.4
- Debian-based OS

No other roles are required.

---

## Role Variables

All configurable variables are in `defaults/main.yml` and OS-specific `vars/Debian.yml`.

### Bash

| Variable | Default | Description |
|----------|---------|-------------|
| `bashrc_path` | `/etc/bash.bashrc` | Path to system-wide bashrc |
| `bash_config` | See defaults | Bash settings (HISTSIZE, HISTFILESIZE, PROMPT_COMMAND, etc.) |

### Chrony

| Variable | Default | Description |
|----------|---------|-------------|
| `ntp_pools` | `['0.ru.pool.ntp.org','1.ru.pool.ntp.org','2.ru.pool.ntp.org']` | List of NTP servers |

### SSH

| Variable | Default | Description |
|----------|---------|-------------|
| `sshd_config` | See defaults | SSH daemon configuration: port, log_level, authentication, etc. |

### Sysctl

| Variable | Default | Description |
|----------|---------|-------------|
| `sysctl_settings` | See defaults | Kernel and system hardening parameters |

### Unattended Upgrades

| Variable | Default | Description |
|----------|---------|-------------|
| `unattended_upgrades` | See `vars/Debian.yml` | Configure automatic security updates and package cleanup |

### Packages

| Variable | Default | Description |
|----------|---------|-------------|
| `base_packages` | See `vars/Debian.yml` | List of essential system packages to install |

---

## Dependencies

This role has no external dependencies.

---

## Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - role: asgladnev.linux-base
      sshd_config_override:
        authentication_methods: "publickey"
      base_packages_extra:
        - tmux
        - mc
        - git
        - xfsprogs
        - qemu-guest-agent
        - cloud-guest-utils
        - ansible
      sysctl_settings_override:
        net.ipv6.conf.all.disable_ipv6: 1
        net.ipv6.conf.default.disable_ipv6: 1
        net.ipv6.conf.lo.disable_ipv6: 1
```

This example will apply the role to all hosts, override bash history size, change SSH port to 2222, and enable automatic reboot after upgrades.

---

## Testing

Test inventory and playbook are provided in `tests/`:

```bash
ansible-playbook -i tests/inventory tests/test.yml --check
```

---

## License

MIT

---

## Author

Alexander Gladnev
Email: gladnev.as@yandex.ru
