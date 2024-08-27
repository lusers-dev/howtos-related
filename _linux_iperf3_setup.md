# Linux Users - Iperf3 Setup

iPerf3 is a widely used open-source tool designed for measuring the performance of computer networks. It allows you to assess the bandwidth and various network-related metrics between two devices, such as a client and a server. iPerf3 is particularly helpful in diagnosing network issues, optimizing network configurations, and testing the capacity of network connections.


`iperf3-setup.txt`

```shell

##### RHEL-8, Amazon Linux 2023 #####

## install iperf3
$ sudo dnf install iperf3

$ sudo iperf3 -s
-----------------------------------------------------------
Server listening on 5201 (test #1)
-----------------------------------------------------------

## add unpriv local iperf3 user
$ sudo useradd iperf3
$ sudo usermod -s /sbin/nologin iperf3

$ sudo grep iperf3 /etc/passwd
iperf3:x:10553:10553::/home/iperf3:/sbin/nologin

## Create a service path: /etc/systemd/system/iperf3.service
## make the listening port 16555 different than default 5201

##########

[Unit]
Description=Start iperf3 service
After=network.target

[Service]
Type=simple
User=iperf3
Group=iperf3
## amazon linux 2023
## ExecStart=/bin/bash -c '/usr/bin/iperf3 -4 -s -p 16555'
## rhel-8
ExecStart=/bin/bash -c '/bin/iperf3 -4 -s -p 16555'

[Install]
WantedBy=multi-user.target

##########

## set iperf3 systemd file permissions
$ sudo chmod 0644 /etc/systemd/system/iperf3.service
$ sudo chown root:root /etc/systemd/system/iperf3.service

## reload 
$ sudo systemctl daemon-reload

## service
$ sudo systemctl enable iperf3.service
$ sudo systemctl start iperf3
$ sudo systemctl restart iperf3

## check status
$ systemctl status iperf3
● iperf3.service - Start iperf3 service
     Loaded: loaded (/etc/systemd/system/iperf3.service; enabled; preset: disabled)
     Active: active (running) since Mon 2023-08-28 10:55:07 EDT; 5s ago
   Main PID: 3107 (iperf3)
      Tasks: 1 (limit: 2257)
     Memory: 644.0K
        CPU: 7ms
     CGroup: /system.slice/iperf3.service
             └─3107 /usr/bin/iperf3 -4 -s -p 16555



## test iperf3
$ iperf3 -c 127.0.0.1 -p 16555 --parallel 2 -i 1 -t 2

Connecting to host 127.0.0.1, port 16555
[  5] local 127.0.0.1 port 51126 connected to 127.0.0.1 port 16555
[  7] local 127.0.0.1 port 51140 connected to 127.0.0.1 port 16555
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-1.00   sec  2.26 GBytes  19.4 Gbits/sec    0   1.06 MBytes       
[  7]   0.00-1.00   sec  2.26 GBytes  19.4 Gbits/sec    0   1.25 MBytes       
[SUM]   0.00-1.00   sec  4.52 GBytes  38.8 Gbits/sec    0             
- - - - - - - - - - - - - - - - - - - - - - - - -
[  5]   1.00-2.00   sec  2.30 GBytes  19.8 Gbits/sec    0   1.06 MBytes       
[  7]   1.00-2.00   sec  2.30 GBytes  19.8 Gbits/sec    0   1.25 MBytes       
[SUM]   1.00-2.00   sec  4.61 GBytes  39.6 Gbits/sec    0             
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-2.00   sec  4.57 GBytes  19.6 Gbits/sec    0             sender
[  5]   0.00-2.00   sec  4.57 GBytes  19.6 Gbits/sec                  receiver
[  7]   0.00-2.00   sec  4.57 GBytes  19.6 Gbits/sec    0             sender
[  7]   0.00-2.00   sec  4.57 GBytes  19.6 Gbits/sec                  receiver
[SUM]   0.00-2.00   sec  9.13 GBytes  39.2 Gbits/sec    0             sender
[SUM]   0.00-2.00   sec  9.13 GBytes  39.2 Gbits/sec                  receiver

iperf Done.
```

_iPerf3 supports various testing modes, such as TCP and UDP testing, which allow you to evaluate network behavior under different conditions. UDP testing is useful for assessing real-time applications' performance, such as VoIP or streaming, where packet loss and jitter can significantly impact quality._

_Remember that iPerf3 provides raw performance measurements, and the results can be influenced by various factors, including the devices' hardware, network congestion, and system load. It's essential to interpret the results in the context of your network environment._



