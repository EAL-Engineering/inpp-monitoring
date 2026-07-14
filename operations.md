## Change to oual user

    sudo machinectl shell oual@

## Restart Grafana

    systemctl --user restart grafana.service

## Check Grafana Status

    systemctl --user status grafana.service

## View Logs

    journalctl --user -u grafana.service

## Update Container

    podman pull grafana/grafana
    systemctl --user restart grafana.service
