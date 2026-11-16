# homelab-ansible-role-telegraf

[![Lint](https://github.com/iamrobertyoung/homelab-ansible-role-telegraf/actions/workflows/lint.yml/badge.svg)](https://github.com/iamrobertyoung/homelab-ansible-role-telegraf/actions/workflows/lint.yml)
[![Release](https://github.com/iamrobertyoung/homelab-ansible-role-telegraf/actions/workflows/release.yml/badge.svg)](https://github.com/iamrobertyoung/homelab-ansible-role-telegraf/actions/workflows/release.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Ansible role for installing and configuring Telegraf metrics collection agent with InfluxDB output.

## Requirements

- Ansible >= 2.15
- Target: Ubuntu (focal, jammy, noble) or Debian (bullseye, bookworm)
- Docker installed on target (for docker group membership)

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `telegraf_service_name` | `telegraf` | Name of the systemd service |
| `telegraf_influxdb_url` | `localhost` | InfluxDB server URL |
| `telegraf_influxdb_organisation` | `my-org` | InfluxDB organisation |
| `telegraf_influxdb_token` | `SECRET123` | InfluxDB authentication token |
| `telegraf_influxdb_bucket` | `my-bucket` | InfluxDB bucket for metrics storage |

## Usage

### Install via requirements.yml

```yaml
- src: git@github.com:iamrobertyoung/homelab-ansible-role-telegraf.git
  scm: git
  version: main
  name: telegraf
```

```bash
ansible-galaxy install -r requirements.yml
```

### Example Playbook

```yaml
- hosts: servers
  become: true
  roles:
    - role: telegraf
```

### Override defaults

```yaml
- hosts: servers
  become: true
  roles:
    - role: telegraf
      vars:
        telegraf_influxdb_url: "https://influxdb.example.com"
        telegraf_influxdb_organisation: "my-org"
        telegraf_influxdb_token: "{{ vault_telegraf_token }}"
        telegraf_influxdb_bucket: "metrics"
```

## What Gets Installed

- Telegraf metrics collection agent from InfluxData repository
- Custom systemd service configuration
- Telegraf configuration for InfluxDB v2 output
- Telegraf user added to docker group for container metrics

## License

MIT
