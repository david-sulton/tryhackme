# Tcpdump: The Basics
![image](https://github.com/user-attachments/assets/a73ec2f5-a373-4286-a04b-89b5587333b5)

## Introduction
- The Tcpdump tool and its libpcap library are written in C and C++ and were released for Unix-like systems in the late 1980s or early 1990s. Consequently, they are very stable and offer optimal speed. The libpcap library is the foundation for various other networking tools today. Moreover, it was ported to MS Windows as winpcap.
## Basic Packet Capture
![image](https://github.com/user-attachments/assets/9e9523b9-2b1e-4e8f-9087-c2c694f89af4)

## Filtering Expressions
### Filtering by Host
- Let’s say you are only interested in IP packets exchanged with your network printer or a specific game server. You can easily limit the captured packets to this host using host IP or host HOSTNAME. In the terminal below, we capture all the packets exchanged with example.com and save them to http.pcap. It is important to note that capturing packets requires you to be logged-in as root or to use sudo.
  - `sudo tcpdump host example.com -w http.pcap`
### Filtering by Port
- If you want to capture all DNS traffic, you can limit the captured packets to those on port 53. Remember that DNS uses UDP and TCP ports 53 by default. In the following example, we can see all the DNS queries read by our network card. The terminal below shows two DNS queries: the first query requests the IPv4 address used by example.org, while the second requests the IPv6 address associated with example.org.
- `sudo tcpdump -i ens5 port 53 -n`
### Filtering by Protocol

- The final type of filtering we will cover is filtering by protocol. You can limit your packet capture to a specific protocol; examples include: ip, ip6, udp, tcp, and icmp. In the example below, we limit our packet capture to ICMP packets. We can see an ICMP echo request and reply, which is a possible indication that someone is running the ping command. There is also an ICMP time exceeded; this might be due to running the traceroute command (as explained in the Networking Essentials room).
![image](https://github.com/user-attachments/assets/2338779f-89a5-4a2c-a885-d3a86d847ecd)
`tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc`
`sudo tcpdump -r traffic.pcap icmp -n | wc`
`sudo tcpdump -r traffic.pcap arp and host 192.168.124.137`
`sudo tcpdump -r traffic.pcap port 53 -A`
`sudo tcpdump -r traffic.pcap 'greater 15000' -n`
## Advanced Filtering
![image](https://github.com/user-attachments/assets/53a6bd8a-8f31-4f18-b6a1-ad2281121aa5)
- `sudo tcpdump -r traffic.pcap 'tcp[tcpflags] == tcp-rst' | wc -l`
## Displaying Packets
![image](https://github.com/user-attachments/assets/b3021fc7-ac0e-4189-a258-173bcaf090ca)
- `sudo tcpdump -r traffic.pcap arp -e`
## Conclusion
