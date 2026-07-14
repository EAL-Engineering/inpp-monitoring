# Deployment

## Overview

The INPP monitoring stack provides:

- InfluxDB for time-series data storage
- Grafana for visualization and dashboards

Services are deployed using:

- Podman (rootless)
- Podman Quadlets
- User-level systemd services

The stack runs under the `oual` account.

## Environment

Host OS:

- Red Hat Enterprise Linux 9.7

Container Runtime:

- Podman 5.8.2

Service Account:

- oual

Container Management:

- Podman Quadlets
- systemd --user

## Prerequisites

### Podman

Verify Podman is installed:

    podman --version

Expected output:

    podman version 5.8.2

### Subordinate UID/GID Ranges

Rootless containers require subordinate UID and GID ranges.

Example configuration:

`/etc/subuid`

    oual:165536:65536

`/etc/subgid`

    oual:165536:65536

Verify mappings:

    podman info

Expected:

    idMappings:
      uidmap:
      - container_id: 0
        host_id: 636
        size: 1
      - container_id: 1
        host_id: 165536
        size: 65536

and

    gidmap:
      - container_id: 0
        host_id: 636
        size: 1
      - container_id: 1
        host_id: 165536
        size: 65536

### User Service Persistence

Verify lingering is enabled:

    loginctl show-user oual

Expected:

    Linger=yes

This allows user services to start automatically after system reboot even when
no user is logged in.

## Directory Layout

Repository:

    ~/inpp-monitoring/

Persistent application data:

    ~/inpp-monitoring/influxdb/data
    ~/inpp-monitoring/grafana/data

Runtime data directories should not be committed to Git.

## InfluxDB Deployment

### Create Data Directory

    mkdir -p ~/inpp-monitoring/influxdb/data

### Install Quadlet

Create:

    ~/.config/containers/systemd/influxdb.container

Contents:

    [Unit]
    Description=INPP InfluxDB
    After=network-online.target

    [Container]
    Image=docker.io/library/influxdb:2

    ContainerName=influxdb

    PublishPort=127.0.0.1:8086:8086

    Volume=%h/inpp-monitoring/influxdb/data:/var/lib/influxdb2:Z

    [Service]
    Restart=always

    [Install]
    WantedBy=default.target

### Reload Systemd

    systemctl --user daemon-reload

### Enable and Start Service

    systemctl --user enable influxdb
    systemctl --user start influxdb

### Check Service Status

    systemctl --user status influxdb

Expected:

    Active: active (running)

### Verify Health

    curl http://127.0.0.1:8086/health

Example response:

    {
      "name":"influxdb",
      "message":"ready for queries and writes",
      "status":"pass"
    }

## Operational Commands

Start:

    systemctl --user start influxdb

Stop:

    systemctl --user stop influxdb

Restart:

    systemctl --user restart influxdb

Status:

    systemctl --user status influxdb

List Containers:

    podman ps

Container Logs:

    podman logs influxdb

## Future Components

The following items have not yet been deployed:

- Grafana
- Apache reverse proxy configuration
- InfluxDB organization
- InfluxDB bucket
- InfluxDB API tokens
- Accelerator telemetry ingestion

These items will be configured after InfluxDB deployment validation.
