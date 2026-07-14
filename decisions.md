# Decisions

Decisions that have been made by various members of accelerator staff.  Keep this like a logbook with the newest entries at the top.

## 2026-07-14

### Container Platform

Decision:
Use Podman instead of Docker.

Reason:
- Native RHEL support.
- Rootless operation.
- Better alignment with future loss of root access.
- Simpler integration with user-level systemd services.

Alternatives Considered:
- Docker
- Kubernetes

Status:
Accepted
