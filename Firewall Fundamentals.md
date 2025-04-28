# Firewall Fundamentals
![image](https://github.com/user-attachments/assets/7fc04a35-4ad2-4ed2-a906-b4ffda8597d4)

- We have seen security guards outside shopping malls, banks, restaurants, and houses. These guards are placed at the entrances of these areas to keep a check on people coming in or going out. The purpose of maintaining this check is to ensure nobody sneaks in without being permitted. This guard acts as a wall between his area and the visitors.

- A lot of incoming and outgoing traffic flows daily between our digital devices and the Internet they are connected to. What if somebody sneaks in between this massive traffic without getting caught? We would also need a security guard for our digital devices then, who can check the data coming in and going out of them. This security guard is what we call a firewall. A firewall is designed to inspect a network's or digital device’s incoming and outgoing traffic. The goal is the same as for the security guard sitting outside a building: not letting any unauthorized visitor enter a system or a network. You instruct the firewall by giving it rules to check against all the traffic. Anything that comes in or goes out of your device or network would face the firewall first. The firewall will allow or deny that traffic based on its maintained rules. Most firewalls today go beyond rule-based filtering and offer extra functionalities to protect your device or network from the outside world. We will discuss all these firewalls and perform practical lab demonstrations on a few.
![image](https://github.com/user-attachments/assets/3bcc1090-c41e-4e55-bd3f-ecc71ab3941e)

# Firewalls	Characteristics
## Stateless firewalls	
- Basic filtering
- No track of previous connections
- Efficient for high-speed networks
## Stateful firewalls
- Recognize traffic by patterns
- Complex rules can be applicable
- Monitor the network connections
## Proxy firewalls
- Inspect the data inside the packets as well
- Provides content filtering options
- Provides application control
- Decrypts and inspects SSL/TLS data packets
## Next-generation firewalls
- Provides advanced threat protection
- Comes with an intrusion prevention system
- Identify anomalies based on heuristic analysis
- Decrypts and inspects SSL/TLS data packets


## The basic components of a firewall rules:
Source address: The machine’s IP address that would originate the traffic.

Destination address: The machine’s IP address that would receive the data.

Port: The port number for the traffic.

Protocol: The protocol that would be used during the communication.

Action: This defines the action that would be taken upon identifying any traffic of this particular nature.

Direction: This field defines the rule’s applicability to incoming or outgoing traffic.

## Actions:
- Allow
- deny
- forward

## Types:
- Inbound rules
- outbound rules
- forward rules
  
## Windows Firewall
![image](https://github.com/user-attachments/assets/652cac7e-8299-41d9-b8d6-22744249b8ed)
- Private networks
- Guest or public networks

![image](https://github.com/user-attachments/assets/e4b3ae08-1196-47d0-9e15-999c82c2ad81)

## Linux UFW:
    sudo ufw enable
    sudo ufw status
    sudo ufw deny 3389
    sudo ufw status numbered
    sudo ufw delete
    


