# Decisions

Decisions that have been made by various members of accelerator staff.  Keep this like a logbook with the newest entries at the top.

## 2026-07-14

### Service Account - GML + Copilot

Decision:
Deploy monitoring infrastructure under `oual`.

Reason:
- Existing institutional account.
- Existing access procedures.
- Existing user-level services.
- Reduces administrative complexity.

### Container Platform - GML + Copilot

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
