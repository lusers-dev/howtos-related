# Linux Users - TFTP Server Setup HowTo

This guide covers the installation and configuration of the TFTP server on Red Hat 9.

Following these steps should set up and configure a TFTP server on your Red Hat 9 system. 
Ensure you have the necessary permissions to execute these commands and modify system configurations.

## Prerequisites

- A system running Red Hat 9
- Sudo or root privileges

## Installation

1. Install the TFTP server package:
   ```sh
   sudo dnf install tftp-server
   ```

## Configuration

1. Enable the TFTP service socket:
   ```sh
   sudo systemctl enable --now tftp.socket
   ```

2. Create a local directory to store the TFTP content and set permissions:
   ```sh
   sudo mkdir -p /local/tftpboot
   sudo chmod 0755 /local/
   sudo chmod 0755 /local/tftpboot
   sudo chown nobody:nobody /local/tftpboot
   ```
   
3. Create a TFTP service unit directory and configure the systemd to allow file uploads:
   ```sh
   sudo mkdir -p /etc/systemd/system/tftp.service.d
   sudo cat > /etc/systemd/system/tftp.service.d/allow_uploads.conf << EOF
   [Service]
   ExecStart=
   ExecStart=/usr/sbin/in.tftpd -c -p  -s /local/tftpboot
   EOF
   ```
   
4. Reload the systemd daemon and restart the TFTP service:
   ```sh
   sudo systemctl daemon-reload
   sudo systemctl restart tftp.socket
   ```



