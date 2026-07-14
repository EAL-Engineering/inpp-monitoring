# Decisions

Decisions that have been made by various members of accelerator staff. Keep this
like a logbook with the newest entries at the top.

## 2026-07-14

### Grafana Reverse Proxy

Issue: Apache proxying resulted in an infinite redirect loop.

Root Cause: Grafana was configured to serve from /grafana/, but Apache stripped
the /grafana prefix before forwarding requests. The last line fixes a problem
where grafana reports origin not allowed when trying to change the password.

Working Configuration:

    ProxyPass /grafana/ http://127.0.0.1:3000/grafana/
    ProxyPassReverse /grafana/ http://127.0.0.1:3000/grafana/
    ProxyPreserveHost On

### Grafana Data Directory Ownership

The official Grafana container runs as:

    uid=472
    gid=0

After creating the data directory, ownership must be adjusted:

    podman unshare chown -R 472:0 ~/inpp-monitoring/grafana/data

Verify:

    podman unshare stat ~/inpp-monitoring/grafana/data

Expected:

    Uid: (472/...)
    Gid: (0/root)

If ownership is not corrected, Grafana fails with:

    GF_PATHS_DATA='/var/lib/grafana' is not writable

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
