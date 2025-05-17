
# Linux Users - rsyslog Server Setup on Red Hat 9

This guide provides the steps necessary to set up an rsyslog server on Red Hat 9 for collecting and managing syslog messages.
Ensure to customize the IP addresses and log file destinations as needed for your environment.

## Prerequisites

- A system running Red Hat 9.
- sudo to root access to the system.

## Step 1: Install rsyslog

First, ensure the `rsyslog` package is installed by running the following command:

```bash
sudo dnf install rsyslog

```

## Step 2: Configure rsyslog

Next, configure rsyslog to listen on UDP port 514 for incoming syslog messages by creating a new configuration file:

```bash
sudo cat > /etc/rsyslog.d/listen.conf << EOF

# Provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

EOF

```

## Step 3: Routing Logs Based on Source IP

To route logs based on the source IP address, add the following rules to `/etc/rsyslog.conf`. Ensure these entries are added right below the **RULES** section:

```bash
#### RULES ####
if \$fromhost-ip=='10.10.10.10' then /var/log/loghost-A_syslog
& stop
if \$fromhost-ip=='10.10.11.11' then /var/log/loghost-B_syslog
& stop

```

## Step 4: Activate rsyslog

Finally, enable and start the rsyslog service to apply the changes:

```bash
sudo systemctl enable --now rsyslog

```



