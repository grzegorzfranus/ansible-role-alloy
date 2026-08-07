# Ansible Role: Grafana Alloy

|Source|Version|CI|License|
|------|-------|--|-------|
|[![Source Code](https://img.shields.io/badge/source-github-blue.svg)](https://github.com/grzegorzfranus/ansible-role-alloy)|[![Version](https://img.shields.io/github/v/release/grzegorzfranus/ansible-role-alloy)](https://github.com/grzegorzfranus/ansible-role-alloy/releases)|[![CI](https://github.com/grzegorzfranus/ansible-role-alloy/actions/workflows/ci.yml/badge.svg)](https://github.com/grzegorzfranus/ansible-role-alloy/actions/workflows/ci.yml)|[![Repository License](https://img.shields.io/badge/license-mit-brightgreen.svg)](LICENSE)|

This Ansible role installs, configures, hardens, and manages Grafana Alloy, the unified telemetry collector for Grafana Loki. It provides a production-ready, secure log shipping solution supporting systemd journald log collection, Docker container log discovery, and mandatory multi-tenant `X-Scope-OrgID` header enforcement.

## ✨ Features

- 📦 **Native Package Deployment**: Installed as a native system package managed directly by systemd
- 🔒 **Multi-Tenant Log Isolation**: Enforces mandatory `alloy_tenant_id` (`X-Scope-OrgID` header) validation
- 📜 **Systemd Journald Ingestion**: Collects system logs via `loki.source.journal` with unit and host relabeling
- 🐳 **Docker Log Discovery**: Automatic container log collection via `discovery.docker` and `loki.source.docker`
- 🛡️ **Systemd Security Sandboxing**: Includes systemd unit security overrides (`ProtectSystem=full`, `ProtectHome=true`)
- 🧪 **Molecule Testing**: Tested via containerized Molecule scenarios (`default`) across supported OS platforms
- 🔄 **Idempotent Lifecycle**: Safe execution supporting `present` and `absent` lifecycle states

## 🎯 Architecture

The role configures Grafana Alloy to run natively on the host system, reading local journal logs and Docker socket logs before shipping them over HTTP/HTTPS to Grafana Loki:

```
[ Systemd Journal / Docker Socket ]
                 │
                 ▼
          [ Grafana Alloy ]
                 │
                 ▼ (HTTP push with X-Scope-OrgID header)
          [ Grafana Loki ]
```

### Delivery Method Decision: Native Package (APT / YUM)

**Architecture Rationale:**
- **Journal & Docker Socket Access:** Alloy acts as an agent reading local `/var/log/journal/` system logs and container logs via `/var/run/docker.sock`. Running Alloy natively on systemd avoids complex volume mounts and socket permissions required when running inside a container.
- **Low Overhead:** Native package execution incurs zero container runtime abstraction overhead and integrates directly with systemd security sandboxing.
- **Automated Lifecycle & Updates:** Pinned native package management (`alloy=1.7.1*`) via official Grafana repositories ensures deterministic, auditable software updates.

## 📋 Requirements

- **Ansible**: 2.15 or higher
- **Python**: 3.9 or higher on target hosts
- **Network**: HTTP/HTTPS connectivity to the central Loki push endpoint (`alloy_loki_url`)
- **Privileges**: sudo/root access on target hosts

### Supported operating systems
List of officially supported operating systems for this role:

| OS Family | Version | Status |
|-----------|---------|---------|
| Ubuntu | 26.04 (Resolute) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Ubuntu | 24.04 (Noble) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Ubuntu | 22.04 (Jammy) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Debian | 13 (Trixie)   | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Debian | 12 (Bookworm) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| Debian | 11 (Bullseye) | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |
| EL (RHEL, Rocky, Alma, Oracle) | 9 | ![✓](https://img.shields.io/badge/✓-brightgreen.svg) |

### Setup module
The role uses facts gathered by Ansible on the remote host (`ansible_facts['os_family']`). If you disable the Setup module in your playbook, the role will not work properly.

### Root access
This role requires root access for package installation and service management.

## 🚀 Quick Start

### 1. Basic Log Shipping Setup

```yaml
---
- name: Deploy Grafana Alloy
  hosts: all
  become: true
  roles:
    - role: grzegorzfranus.alloy
      vars:
        alloy_loki_url: "http://100.95.91.122:3100/loki/api/v1/push"
        alloy_tenant_id: "nonprod"
        alloy_enable_journald_logs: true
```

### 2. Run the playbook

```bash
ansible-playbook -i inventory alloy-setup.yml
```

## ⚙️ Configuration

### Default Configuration

The role comes with production-ready defaults:

```yaml
alloy_state: "present"
alloy_version: "1.7.1"
alloy_become: true
alloy_service_enabled: true
alloy_manage_service_restart: true
alloy_user: "alloy"
alloy_group: "alloy"
alloy_enable_journald_logs: true
alloy_enable_docker_logs: false
alloy_systemd_hardening_enabled: true
```

## 📊 Variables

### Lifecycle Options

| Variable | Description | Default |
|----------|-------------|---------|
| `alloy_state` | Define lifecycle state (`present` or `absent`) | `"present"` |

### Version & Package Repository Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `alloy_version` | Pinned Grafana Alloy release version string | `"1.7.1"` |
| `alloy_repo_gpg_key` | GPG key URL for Grafana package repository | `"https://apt.grafana.com/gpg.key"` |
| `alloy_apt_repo` | APT repository specification line for Grafana packages | `"deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main"` |
| `alloy_yum_repo_baseurl` | YUM/DNF repository base URL for Grafana packages | `"https://rpm.grafana.com"` |

### General Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `alloy_become` | Enable privilege escalation for tasks | `true` |
| `alloy_service_enabled` | Whether to enable and start Alloy service | `true` |
| `alloy_manage_service_restart` | Restart service on configuration change | `true` |
| `alloy_user` | System user account for Alloy daemon | `"alloy"` |
| `alloy_group` | System group account for Alloy daemon | `"alloy"` |

### Loki Push Target & Multi-Tenancy

| Variable | Description | Default |
|----------|-------------|---------|
| `alloy_loki_url` | **Mandatory** Loki push endpoint URL | `""` |
| `alloy_tenant_id` | **Mandatory** Loki tenant ID (`X-Scope-OrgID` header) | `""` |
| `alloy_custom_labels` | Custom extra labels attached to shipped logs | `{}` |

### Log Collection Sources

| Variable | Description | Default |
|----------|-------------|---------|
| `alloy_enable_journald_logs` | Enable systemd journald log collection | `true` |
| `alloy_enable_docker_logs` | Enable Docker container log collection | `false` |
| `alloy_docker_socket_path` | Unix socket path to Docker daemon | `"unix:///var/run/docker.sock"` |

### Systemd Hardening & Execution

| Variable | Description | Default |
|----------|-------------|---------|
| `alloy_systemd_hardening_enabled` | Enable systemd security sandboxing override | `true` |

## 📌 Role Properties

| Property | Value | Description |
|----------|-------|-------------|
| **Idempotent** | ✅ Yes | Running the role multiple times with the same parameters produces the same result. |
| **Atomic** | ❌ No | The role can be partially applied. A failure mid-execution may leave the system in an intermediate state. |
| **Check Mode** | ✅ Supported | Most tasks work in check mode. Mutating commands are skipped. |
| **Diff Mode** | ✅ Supported | Template tasks support diff mode for change preview. |

## 📤 Role Output

This role does not set any public output facts.

## 🔍 Verification

After deployment, verify that Grafana Alloy is shipping logs:

### Check Alloy Status

```bash
# Check service status
sudo systemctl status alloy

# Check configuration syntax
sudo alloy fmt /etc/alloy/config.alloy
```

### Check Log Ingestion via Loki API

```bash
curl -s -G "http://100.95.91.122:3100/loki/api/v1/query_range" \
  -H "X-Scope-OrgID: nonprod" \
  --data-urlencode 'query={host="app-npd-01.comlia.app"}' \
  --data-urlencode "start=$(( $(date +%s) - 900 ))000000000" \
  --data-urlencode "end=$(date +%s)000000000"
```

## 🛡️ Security Features

- ✅ **Mandatory Multi-Tenant Validation**: Enforces non-empty `alloy_tenant_id` before rendering config
- ✅ **Secure File Permissions**: `config.alloy` rendered with `0640` permissions owned by `root:alloy`
- ✅ **Systemd Hardening**: Applies `ProtectSystem=full`, `ProtectHome=true`, and `PrivateTmp=true` overrides
- ✅ **Least Privilege Access**: `alloy` user granted read access to `systemd-journal` and `docker` groups only as needed

### Uninstall

Automated removal is supported via `alloy_state: absent`. To remove Alloy manually:

```bash
sudo systemctl disable --now alloy
sudo apt remove --purge alloy   # or: sudo dnf remove alloy
```

### Roll-back Capabilities

Configuration files can be restored from system backups or previous git tags. If reverting configuration, restart the `alloy` systemd service.

## 🔒 Security considerations

- Keep `alloy_tenant_id` synchronized with your environment structure (`management`, `nonprod`, `production`).
- Ensure Loki endpoint (`alloy_loki_url`) is bound to a secure mesh network (Tailscale) or protected by TLS.

## 🧪 Check mode behavior

- Configuration template rendering and file permission checks run normally in Check Mode.
- Mutating package installation and systemd service changes are safely skipped.

## 🏷️ Tags usage

- Use `--tags` to run selective parts of the role: `always`, `alloy_setup`, `alloy_init`, `alloy_validate`, `alloy_requirements`, `alloy_install`, `alloy_configure`, `alloy_service`, `alloy_remove`.

## 🌐 Network resilience

- Alloy pushes logs over HTTP/HTTPS to `alloy_loki_url`. Ensure firewall and routing rules allow outbound TCP traffic to the Loki port (e.g. `3100`).

## 🧰 Repository management

- This role configures official Grafana package repositories (`https://apt.grafana.com` on Debian/Ubuntu, `https://rpm.grafana.com` on RedHat/EL).

## 🔧 Troubleshooting

### Check Service Logs

```bash
# View systemd journal for Alloy
sudo journalctl -u alloy -f --no-pager
```

## 📁 File Structure

```
ansible-role-alloy/
├── defaults/
│   └── main.yml             # Default configuration variables
├── handlers/
│   └── main.yml             # Systemd reload and service restart handlers
├── meta/
│   ├── main.yml             # Role metadata and Galaxy specifications
│   └── argument_specs.yml   # Native argument specification validation
├── molecule/
│   └── default/             # Default testing scenario
│       ├── converge.yml
│       ├── molecule.yml
│       ├── prepare.yml
│       └── verify.yml
├── tasks/
│   ├── main.yml             # Main task orchestration
│   ├── assert.yml           # Preflight parameter assertions
│   ├── prerequisites.yml    # APT/YUM repository & GPG setup
│   ├── install.yml          # Package installation
│   ├── configure.yml        # Config & systemd override deployment
│   ├── service.yml          # Service state management
│   └── remove.yml           # Removal and cleanup tasks
├── templates/
│   ├── config.alloy.j2      # Main Alloy HCL configuration template
│   └── override.conf.j2     # Systemd hardening override template
└── vars/
    ├── main.yml             # Internal constants
    ├── debian.yml           # Debian/Ubuntu package variables
    └── redhat.yml           # RedHat/EL package variables
```

## 🏷️ Tags

All tags are prefixed with `alloy_` to avoid collisions (except standard `always`).

| Tag | Description |
|-----|-------------|
| `always` | Tasks that always run (OS variable gathering and validation assertions) |
| `alloy_setup` | Grouping tag for all setup tasks (prerequisites, installation, configuration, service management) |
| `alloy_init` | Initial prerequisite and repository configuration tasks |
| `alloy_validate` | Variable assertion and preflight validation tasks |
| `alloy_requirements` | System package and GPG repository requirements setup |
| `alloy_install` | Package installation and user group membership tasks |
| `alloy_configure` | Service configuration and systemd hardening override tasks |
| `alloy_service` | Service state management and startup tasks |
| `alloy_remove` | Uninstallation and directory cleanup tasks |

## CI/CD Pipeline

This repository uses automated quality gates enforcing YAML linting, Ansible linting, and Molecule container integration testing.

## Example Playbooks

### Production Multi-Tenant Setup with Docker & Custom Labels

```yaml
---
- name: Deploy Grafana Alloy for Production Workloads
  hosts: logging_clients
  become: true
  roles:
    - role: grzegorzfranus.alloy
      vars:
        alloy_loki_url: "http://100.95.91.122:3100/loki/api/v1/push"
        alloy_tenant_id: "production"
        alloy_enable_journald_logs: true
        alloy_enable_docker_logs: true
        alloy_custom_labels:
          environment: "production"
          datacenter: "poland-dc1"
          cluster: "k8s-edge"
```

## 🤝 Contributing

Contributions, bug reports, and feature requests are welcome!

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👥 Author Information

This role was created by [Grzegorz Franus](https://github.com/grzegorzfranus).