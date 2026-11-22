# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Ansible role (`telegraf`) for installing and configuring the Telegraf metrics collection agent on Ubuntu/Debian systems. Sends metrics to InfluxDB v2.

## Development Commands

```bash
# Linting
yamllint .                    # Lint all YAML files
ansible-lint                  # Lint Ansible code

# Pre-commit
pre-commit install            # Install hooks (one-time setup)
pre-commit run --all-files    # Run all hooks manually

# Molecule testing (Docker-based)
molecule test          # Run full test suite
molecule converge      # Apply role to test container
molecule verify        # Run verification only
molecule login         # Access container for debugging
molecule destroy       # Clean up test containers

# Tool management
mise install           # Install tool versions (Ansible 13, pipx 1.8)
```

## Role Variables

All have defaults in `defaults/main.yml`. Override `telegraf_influxdb_token` with a valid token.

| Variable | Default | Description |
|----------|---------|-------------|
| `telegraf_influxdb_token` | `SECRET123` | InfluxDB authentication token |
| `telegraf_influxdb_url` | `localhost` | InfluxDB server URL |
| `telegraf_influxdb_organisation` | `my-org` | InfluxDB organisation |
| `telegraf_influxdb_bucket` | `my-bucket` | InfluxDB bucket |
| `telegraf_docker_input_enabled` | `true` | Enable Docker metrics input plugin |

## Architecture

- `tasks/main.yml` - Adds InfluxData repo, installs Telegraf, deploys config/service, adds the telegraf user to the docker group when it exists, flushes handlers, ensures the service is enabled and started
- `handlers/main.yml` - One restart handler that reloads systemd and restarts the service; no chained handlers, since handler names are global to the play
- `templates/telegraf.conf.j2` - Telegraf config with conditional docker input plugin
- `templates/telegraf.service.j2` - Systemd unit file
- `molecule/default/prepare.yml` - Installs `python3-debian` for `deb822_repository` module

## YAML Style

Line length max 120, truthy values `true`/`false` or `yes`/`no`, 1 space minimum after comments.

## Commit Convention

Uses [Conventional Commits](https://www.conventionalcommits.org/) for semantic-release:
- `feat:` → minor, `fix:` → patch, `docs:`/`refactor:`/`perf:` → patch
- `chore:`/`test:`/`ci:` → no release
