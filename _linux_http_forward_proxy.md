# Linux Users - HowTo Setup an HTTP Forward Proxy

With these steps, you can install and configure an HTTP forward proxy server and enabled clients to use the proxy for OS patching purposes.

The http-forward proxy will be configured to listen on port 1080 and allow access to systems from the 10.10.10.0/24 subnet.

### Server Installation and Configuration

1. Install  Apache HTTP Web Server (httpd). 

```bash
sudo dnf install httpd
```

2. Configure httpd to listen on port 1080 instead of the default port 80. 

Edit the `/etc/httpd/conf/httpd.conf` file and add the following line:

```bash
Listen 1080
```

3. Create a new configuration file `/etc/httpd/conf.d/001-http-proxy.conf` to enable http-foward proxy functionality:

```bash
sudo cat > /etc/httpd/conf.d/001-http-proxy.conf << EOF

<VirtualHost *:*>
    ProxyRequests On
    ProxyVia On

    <Proxy *:*>
        Require ip 10.10.10.0/24
    </Proxy>
</VirtualHost>

EOF
```

This configuration enables the proxy server and restricts access to the 10.10.10.0/24 subnet.

4. Enable and start the Apache HTTP Web Server:

```bash
sudo systemctl enable --now httpd
systemctl status httpd
```

### Client Configuration

To configure Red Hat clients with no direct internet access and to use the HTTP forward proxy for OS patching, follow these steps:

1. Edit the `/etc/rhsm/rhsm.conf` file and add the following lines:

```bash
proxy_hostname=10.10.11.11
proxy_port=1080
```

Replace `10.10.11.11` with the IP address of your HTTP forward proxy server.

2. Register the system with Red Hat Network (RHN):

```bash
sudo subscription-manager register --username <rhn_user> --auto-attach --force
```

3. Enable the required repositories for your Red Hat 9 system:

```bash
sudo subscription-manager repos --enable rhel-9-for-x86_64-baseos-rpms --enable rhel-9-for-x86_64-supplementary-rpms
```

4. Check for updates and apply them:

```bash
dnf check-update
sudo dnf update
```


