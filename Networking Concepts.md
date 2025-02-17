# Networking Concepts
- https://tryhackme.com/room/networkingconcepts

## Intro
### Learning Objectives
- ISO OSI network model
- IP addresses, subnets, and routing
- TCP, UDP, and port numbers
- How to connect to an open TCP port from the command line

## OSI model
- Layer 7 	Application layer 	Providing services and interfaces to applications 	HTTP, FTP, DNS, POP3, SMTP, IMAP
- Layer 6 	Presentation layer 	Data encoding, encryption, and compression 	Unicode, MIME, JPEG, PNG, MPEG
- Layer 5 	Session layer 	Establishing, maintaining, and synchronising sessions 	NFS, RPC
- Layer 4 	Transport layer 	End-to-end communication and data segmentation 	UDP, TCP
- Layer 3 	Network layer 	Logical addressing and routing between networks 	IP, ICMP, IPSec
- Layer 2 	Data link layer 	Reliable data transfer between adjacent nodes 	Ethernet (802.3), WiFi (802.11)
- Layer 1 	Physical layer 	Physical data transmission media 	Electrical, optical, and wireless signals

## TCP/IP Model
![image](https://github.com/user-attachments/assets/454b64f1-04d7-4ca4-914f-0abfe3e4423d)
		

## IP addresses an Subnets
![image](https://github.com/user-attachments/assets/f23f36ed-520a-43b4-b830-26531889c10a)
### Private IP addresses ranges
- 10.0.0.0–10.255.255.255
- 172.16.0.0–172.31.255.255
- 192.168.0.0–192.168.255.255
- ```ipconfig```
## UDP and TCP
- TCP(3 way handshake) is a reliable, connection-oriented protocol that ensures data is delivered in order, with error checking and retransmission for lost packets. It’s slower due to its overhead and is used for applications where data integrity is crucial (e.g., web browsing, file transfer).
![image](https://github.com/user-attachments/assets/5729c81f-82ed-4048-8d86-73df71df27bd)

- UDP, on the other hand, is a faster, connectionless protocol that does not guarantee delivery or order. It has less overhead and is used for real-time applications where speed is more important than reliability (e.g., streaming, online gaming).
## Encapsulation
- Encapsulation is an essential concept as it allows each layer to focus on its intended function. In the image below, we have the following four steps:
- Application data: It all starts when the user inputs the data they want to send into the application. For example, you write an email or an instant message and hit the send button. The application formats this data and starts sending it according to the application protocol used, using the layer below it, the transport layer.
- Transport protocol segment or datagram: The transport layer, such as TCP or UDP, adds the proper header information and creates the TCP segment (or UDP datagram). This segment is sent to the layer below it, the network layer.
- Network packet: The network layer, i.e. the Internet layer, adds an IP header to the received TCP segment or UDP datagram. Then, this IP packet is sent to the layer below it, the data link layer.
- Data link frame: The Ethernet or WiFi receives the IP packet and adds the proper header and trailer, creating a frame.

## Telnet
- The TELNET (Teletype Network) protocol is a network protocol for remote terminal connection. In simpler words, telnet, a TELNET client, allows you to connect to and communicate with a remote system and issue text commands. Although initially it was used for remote administration, we can use telnet to connect to any server listening on a TCP port number.
```telnet MACHINE_IP 80```
```GET / HTTP/1.1```
```Host: telnet.thm```

## Conclusion
- In this room, we covered the ISO OSI and TCP/IP models, comparing and contrasting the two. We also covered IP addresses and subnets, briefly explaining routing. Furthermore, after diving into TCP and UDP, we explained encapsulation. For demonstration purposes, we used telnet to “talk” to different servers over TCP.

