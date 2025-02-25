![image](https://github.com/user-attachments/assets/a5eb6093-6b3c-4dc8-b917-ffb8e6b6f00b)

# Networking Essentials

## Intro
- Learning Objectives

- The objective of this room is to teach you about various standard protocols and technologies that glue things together:

      Dynamic Host Configuration Protocol (DHCP)
      Address Resolution Protocol (ARP)
      Network Address Translation (NAT)
      Internet Control Message Protocol (ICMP)
        Ping
        Traceroute

## DHCP: Give me my network settings
### DHCP follows four steps: Discover, Offer, Request, and Acknowledge (DORA):

    DHCP Discover: The client broadcasts a DHCPDISCOVER message seeking the local DHCP server if one exists.
    DHCP Offer: The server responds with a DHCPOFFER message with an IP address available for the client to accept.
    DHCP Request: The client responds with a DHCPREQUEST message to indicate that it has accepted the offered IP.
    DHCP Acknowledge: The server responds with a DHCPACK message to confirm that the offered IP address is now assigned to this client.

![image](https://github.com/user-attachments/assets/47dfdc2d-e906-4635-8751-b0382f0ae2f6)

## ARP: Bridging Layer 3 Addressing to Layer 2 Addressing
- Address Resolution Protocol (ARP) makes it possible to find the MAC address of another device on the Ethernet.

## ICMP: Troubleshooting Networks
- Internet Control Message Protocol (ICMP) is mainly used for network diagnostics and error reporting. Two popular commands rely on ICMP, and they are instrumental in network troubleshooting and network security. The commands are:

          ping: This command uses ICMP to test connectivity to a target system and measures the round-trip time (RTT). In other words, it can be used to learn that the target is alive and that its reply can reach our system.
          traceroute: This command is called traceroute on Linux and UNIX-like systems and tracert on MS Windows systems. It uses ICMP to discover the route from your host to the target.
- The terminal output below shows the result of running traceroute to discover the routers between our system and example.com. Some routers don’t respond; in other words, they drop the packet without sending any ICMP messages. Routers that belong to our ISP might respond, revealing their private IP address. Moreover, some routers respond and show their public IP address, and this would let us look up their domain name and discover their geographic location. Finally, there is always a possibility that an ICMP Time Exceeded message gets blocked and never reaches us.
## Routing

    OSPF (Open Shortest Path First): OSPF is a routing protocol that allows routers to share information about the network topology and calculate the most efficient paths for data transmission. It does this by having routers exchange updates about the state of their connected links and networks. This way, each router has a complete map of the network and can determine the best routes to reach any destination.
    EIGRP (Enhanced Interior Gateway Routing Protocol): EIGRP is a Cisco proprietary routing protocol that combines aspects of different routing algorithms. It allows routers to share information about the networks they can reach and the cost (like bandwidth or delay) associated with those routes. Routers then use this information to choose the most efficient paths for data transmission.
    BGP (Border Gateway Protocol): BGP is the primary routing protocol used on the Internet. It allows different networks (like those of Internet Service Providers) to exchange routing information and establish paths for data to travel between these networks. BGP helps ensure data can be routed efficiently across the Internet, even when traversing multiple networks.
    RIP (Routing Information Protocol): RIP is a simple routing protocol often used in small networks. Routers running RIP share information about the networks they can reach and the number of hops (routers) required to get there. As a result, each router builds a routing table based on this information, choosing the routes with the fewest hops to reach each destination.

## NAT
![image](https://github.com/user-attachments/assets/4a35d349-d587-44d6-8742-5eadd51c062a)

- The idea behind NAT lies in using one public IP address to provide Internet access to many private IP addresses. In other words, if you are connecting a company with twenty computers, you can provide Internet access to all twenty computers by using a single public IP address instead of twenty public IP addresses. (Note: Technically speaking, the number of IP addresses is always expressed as a power of two. To be technically accurate, with NAT, you reserve two public IP addresses instead of thirty-two. Consequently, you would have saved thirty public IP addresses.)

## Closing Notes
