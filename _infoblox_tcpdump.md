# Linux Users - Infoblox Expert Mode and Tcpdump

`set expertmode` and `tcpdump` 

The command sequence below relates to the Infoblox CLI. This sequence enters Infoblox Expert Mode and then uses `tcpdump` to capture DNS traffic on the `eth1` network interface, filtering for UDP packets on port 53. The captured data is then passed to `grep` to search for occurrences of "xyz.foobar.com" within the DNS traffic, helping to analyze or troubleshoot DNS-related issues involving that domain.

`infoblox-expertmode-tcpdump.txt`

```shell
$ ssh admin-user@10.10.5.5

Warning: Permanently added '10.10.5.5' (RSA) to the list of known hosts.

Infoblox > show network 
Current LAN1 Network Settings:
  IPv4 Address:               10.10.8.8
  Network Mask:               255.255.254.0
  Gateway Address:            10.10.8.1
  VLAN Tag:                   Untagged
  HA enabled:                 false
  Grid Status:                Member of ND Grid Grid

Current Management Network Settings:
  Management Port enabled:        true
  Management IPv4 Address:        10.10.5.5
  Management Netmask:             255.255.254.0
  Management Gateway Address:     10.10.5.1
  Restrict Support and remote console access to MGMT port:        true

Infoblox > 
Infoblox > set expertmode

Expert Mode > tcpdump -i eth1 -n -s0 '((udp) and (port 53))' | grep xyz.foobar.com
16:37:58.531904 IP 192.168.1.244.45823 > 10.10.8.8.53: 3914+ [1au] A? xyz.foobar.com. (55)
16:37:58.532351 IP 10.10.8.8.47769 > 8.8.8.8.53: 62779+% [1au] A? xyz.foobar.com. (43)
^C
Expert Mode > exit

```

Let's break down these commands step by step:

1. `Infoblox > set expertmode`
   - This command switches the Infoblox CLI (Command Line Interface) to "Expert Mode." In Expert Mode, you typically have access to more advanced and potentially powerful commands and configurations, which can be useful for troubleshooting and performing tasks that require deeper network analysis.

2. `Expert Mode > tcpdump -i eth1 -n -s0 '((udp) and (port 53))' | grep xyz.foobar.com`
   - This is a bit more complex command that utilizes the `tcpdump` and `grep` utilities to capture and filter network traffic.

   - `tcpdump`: This is a packet capture tool commonly used for network analysis. Here's a breakdown of its options and arguments used in the command:
     - `-i eth1`: Specifies the network interface to capture traffic from (in this case, `eth1`).
     - `-n`: Prevents DNS resolution of IP addresses to hostnames. This keeps the output more focused on IP addresses and ports.
     - `-s0`: Sets the snaplen to 0, which means to capture the entire packet. This ensures that all data in the captured packets is visible.
     - `((udp) and (port 53))`: This is a BPF (Berkeley Packet Filter) expression used to filter packets. It specifies that we are interested in UDP packets on port 53, which is the standard port for DNS traffic.

   - `|`: This is a pipe symbol used to pass the output of one command as input to another.

   - `grep xyz.foobar.com`: After capturing the network traffic with `tcpdump`, this part of the command pipes the captured data to the `grep` command, which searches for the text "xyz.foobar.com" within the captured packets. This can be useful for identifying DNS queries or responses related to the domain "xyz.foobar.com."



