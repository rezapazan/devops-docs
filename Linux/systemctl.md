# Systemd

## `systemctl` Command

```bash
$ systemctl status [service_name]
# check status
$ systemctl is-enabled [service_name]
# if the service starts on OS boot
$ systemctl disable [service_name]
# disable autostart on boot
$ systemctl edit [service_name]
# to edit systemd config of the service
$ systemctl mask | unmask [service_name]
```

**Note:** You can edit systemd config file directly, address can be found using `systemctl status` under loaded section (e.g. `vim /usr/lib/systemd/system/nginx.service`).

## `journalctl` Command

```bash
$ journalctl -xefu nginx
# -f follows, -u for unit name
```
