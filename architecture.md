# Architecture

## Overview

The INPP monitoring system consists of:

1. Data acquisition systems
2. InfluxDB time-series database
3. Grafana visualization platform

The monitoring stack (items 2 and 3) runs on the INPP RHEL 9.7 virtual machine, hosted by OIT.

## Service Relationships

Accelerator Hardware
        |
        v
Data Collection Software
        |
        v
InfluxDB
        |
        v
Grafana
        |
        v
Users

## Container Strategy

Services are deployed as rootless Podman containers.

Containers execute under the `oual` user account.

Containers are managed through user-level systemd services.

The system does not require root access for routine operation.

## Startup Model

The `oual` account has systemd linger enabled.

As a result:

- services start automatically at boot
- services continue running without interactive login
- system recovery does not require user intervention

## Networking

User Access
       |
       v
Apache HTTPS Proxy
       |
       v
Grafana Container

InfluxDB is intended to be accessible only to authorized
internal applications.
