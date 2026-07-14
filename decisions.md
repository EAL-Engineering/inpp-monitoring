# Decisions

Decisions that have been made by various members of accelerator staff. Keep this
like a logbook with the newest entries at the top.

## 2026-07-14

### Data Storage Location - GML + Copilot

Decision: Store Grafana and InfluxDB persistent data under the oual home
directory.

Layout:

    ~/inpp-monitoring/grafana/data
    ~/inpp-monitoring/influxdb/data

Reason:

- Does not require root access.
- Included in normal account backup procedures.
- Simplifies migration to future systems.
- Keeps infrastructure self-contained.

### Podman - GML + Copilot

Decision: Deploy production services using Podman Quadlets managed by user-level
systemd. Add oual to /etc/subuid and /etc/subgid. New files read:

    rexuser:100000:65536
    oual:165536:65536

Reasons:

    - Native Podman functionality.
    - Native RHEL support.
    - Direct systemd integration.
    - Consistent with existing elog.service management.

Implementation Note:13A compose.yaml may be maintained in the repository for
documentation,14development, and conversion purposes.

### Service Account - GML + Copilot

Decision: Deploy monitoring infrastructure under `oual`.

Reason:

- Existing institutional account.
- Existing access procedures.
- Existing user-level services.
- Reduces administrative complexity.

### Container Platform - GML + Copilot

Decision: Use Podman instead of Docker.

Reason:

- Native RHEL support.
- Rootless operation.
- Better alignment with future loss of root access.
- Simpler integration with user-level systemd services.

Alternatives Considered:

- Docker
- Kubernetes

Status: Accepted
