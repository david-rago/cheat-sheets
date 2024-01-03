# Usage
## Enumerating Targets (Host Discovery)
* **list**: scan targets specified `nmap scanme.nmap.org github.com` will scan 2 IP addresses.
* **range**: scan range specified `nmap 10.0.0.1-4` will scan `10.0.0.1`, `10.0.0.2`, `10.0.0.3` and `10.0.0.4`.
* **subnet**: scan the subnet `nmap 10.0.0.1/30` will scan `10.0.0.0`, `10.0.0.1`, `10.0.0.2` and `10.0.0.3`.
It's possible to use a list as input of targets `nmap -iL list.txt`.
If you want check the targets the nmap utility will scan (without scanning them) we can use `nmap -sL TARGETS`. It gives you useful info as the number of total hosts.
Nmap will try to do reverse-DNS resolution to obtain the name, to avoid that we can add `-n`.
`nmap -sn` **disable port scan**. We will use a lot this option when performing host discovery, we don't want to waste a lot of time checking port status of offline hosts.
`nmap -PR` will do ARP scan, useful when doing a scan in a local network.
### ICMP scan
Nmap ping scan options:
* `-PE` ICMP Echo
* `-PP` ICMP Timestamp
* `-PM` ICMP Address Mask
It's good to know the are different types of ICMP scan, since a lot of time firewalls block one type but maybe not the other one. The most common one of these is the Echo one.
### TCP and UDP Ping scan
#### TCP SYN Ping
We use the `-PS` to specify a TCP SYN ping. This type of scan will give a response, if the port is open the target will reply with ACK, if the port is closed, the target will reply with RST, if no reply then that means machine is down.
This type of scan does not require a privileged account.
#### TCP ACK Ping
Uses `-PA` to specify a TCP ACK Ping. A TCP packet with an ACK flag should get a TCP packet back with an RST flag set, since ACK isn't part of an ongoing connection. If the target reply that means is up, if not then it's down.
This type of scan requires a privileged account. With an unprivileged user, Nmap will attempt a 3-way handshake.
#### UDP Ping
This type of scan doesn't actually reply anything if the port is open, so when doing this type of scan it's better to use rare port numbers. But if the port is closed the target will reply with ICMP port unreachable packet.
We have to add `-PU` to use this type of scan.
## Port scanning
Once the enumeration of targets (host-discovery stage) is done we an procede to do the port scan. It's important to do first the host-discovery stage before attempting to do port scanning, since port scanning offline hosts will waste a lot of time.
* TCP Connect Scan: `-sT`
* TCP SYN Scan: `-sS` Similar to `-sT` but it will RST,ACK the connection without completing the TCP handshake.
* UDP Scan: `-sU`
To select the number of ports we have 3 ways:
* list: `-p80,443,8080`
* range: `-p1-1023` will scan ports between 1 and 1023
* all ports: `-p-` will scan all 65535 possible ports
## Scan Timing
Important to play with this setting to avoid IDS alerts.
There are 6 levels:
* paranoid (0)
* sneaky (1)
* polite (2)
* normal (3)
* aggressive (4)
* insane (5)
By default nmap uses `-T3`, but it's common to see people put it at `-T4` for testing.
But in a real environment the common one is `-T1`. We can also use `-T0` but keep in mind that between each port scan it takes 5 minutes.