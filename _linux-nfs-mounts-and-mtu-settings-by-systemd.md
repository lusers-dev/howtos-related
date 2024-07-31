
# Linux Users - Configuring NFS Mounts and MTU Settings Using systemd

This guide outlines the steps required to configure NFS mounts and adjust MTU settings on a Linux system using systemd.

## Prerequisites

- Install the necessary NFS utilities:

```bash
dnf install nfs-utils
```

## Configuration Steps

### 1. Create a Target Directory

Create the directory where the NFS will be mounted:

```bash
mkdir -p /local/foobar
```

### 2. Configure MTU Settings

Create a systemd service to set the MTU value for your network interface (`ens5` in this example):

1. Open or create the service file:

```bash
vi /etc/systemd/system/set-ens5MTU.service
```

2. Add the following content to the file:

```ini
[Unit]
Description=Set ens5 MTU to 8500
After=network-online.target

[Service]
Type=oneshot
User=root
ExecStart=/bin/bash -c '/usr/sbin/ip link set dev enf5 mtu 8500'

[Install]
WantedBy=multi-user.target
```

3. Reload the systemd daemon and enable the service:

```bash
systemctl daemon-reload
systemctl enable --now set-ens5MTU.service
```

4. Reboot the system to verify changes:

```bash
reboot
```

### 3. Mount the NFS Share

Configure a systemd mount unit to automatically mount the NFS share:

1. Open or create the mount unit file:

```bash
vi /etc/systemd/system/local-foobar.mount
```

2. Add the following content to the file:

```ini
[Unit]
Description=foobar NFS mount on fsxontapnd 
Before=remote-fs.target
After=set-ens5MTU.service

[Mount]
What=10.10.11.11:/test
Where=/local/foobar
Type=nfs
TimeoutSec=30s
Options=rw,bg,proto=tcp

[Install]
WantedBy=remote-fs.target
```

3. Reload the systemd daemon and enable the NFS mount:

```bash
systemctl daemon-reload
systemctl enable --now local-foobar.mount
```

4. Check the mount status:

```bash
df -Th
```

5. Reboot the system to ensure everything is working as expected:

```bash
sudo systemctl reboot
```

## Verification

After rebooting, verify that the MTU settings are applied and the NFS share is mounted correctly by checking your network interface settings and mount points.



