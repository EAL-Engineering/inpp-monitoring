# INPP Monitoring Infrastructure

This repository contains the deployment and operational documentation for
the Institute for Nuclear and Particle Physics (INPP) monitoring
infrastructure.

The system provides:

- Real-time visualization using Grafana
- Time-series storage using InfluxDB
- Rootless container deployment using Podman
- User-level systemd service management under the `oual` account

## Goals

- Minimal dependence on OIT intervention
- Survive VM reboots automatically
- Remain maintainable after staff transitions
- Keep infrastructure reproducible and documented

## Current Environment

Host OS:
- Red Hat Enterprise Linux 9.7

Container Runtime:
- Podman (planned)

Service Account:
- oual

Public Services:
- Wiki
- ELOG
- Grafana (planned)
- InfluxDB (planned)

## Repository Documents

Architecture.md
    High-level system design

Deployment.md
    Installation instructions

Operations.md
    Day-to-day maintenance

Backup-and-Recovery.md
    Backup procedures and disaster recovery

Decisions.md
    Engineering decisions and rationale
