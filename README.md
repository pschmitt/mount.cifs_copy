# mount.cifs for CoreOS Container Linux

## Usage

```bash
docker run -it --rm -v /opt/bin:/target pschmitt/mount.cifs_copy
```

## Mount Hetzner Storage Box

```
[Unit]
Description=Mount Storage Box
After=network.target

[Service]
Type=oneshot
RemainAfterExit=yes
TimeoutStartSec=0
Environment=BOX_USERNAME=uXXXXXX
Environment=BOX_PASSWORD=XXXXXXX
Environment=BOX_MOUNTPOINT=/mnt/storagebox
ExecStartPre=/usr/bin/docker run -v /opt/bin:/target pschmitt/mount.cifs_copy
ExecStart=/opt/bin/mount.cifs -o user=${BOX_USERNAME},pass=${BOX_PASSWORD},x-systemd.device-timeout=30,_netdev //${BOX_USERNAME}.your-storagebox.de/backup ${BOX_MOUNTPOINT}
ExecStop=/usr/bin/umount ${BOX_MOUNTPOINT}

[Install]
WantedBy=multi-user.target
```

## Alternatives and similar work

- [Upstream Issue](https://github.com/coreos/bugs/issues/571)
- https://github.com/so0k/mount.cifs_copy
- [Bind mounting /sbin](https://gist.github.com/monaka/5cb1f33e5317e29285843f158a387c9b)
