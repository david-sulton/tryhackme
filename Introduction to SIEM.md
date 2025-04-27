# Introduction to SIEM
![image](https://github.com/user-attachments/assets/16ae079b-3f27-49a6-ba33-a7674ce4d455)

# What is SIEM

- SIEM stands for Security Information and Event Management system. It is a tool that collects data from various endpoints/network devices across the network, stores them at a centralized place, and performs correlation on them. This room will cover the basic concepts required to understand SIEM and how it works.

![image](https://github.com/user-attachments/assets/cb78199c-14ed-47f7-aa9d-df2675ce3e8f)

# 1) Host-Centric Log Sources

These are log sources that capture events that occurred within or related to the host. Some log sources that generate host-centric logs are Windows Event logs, Sysmon, Osquery, etc. Some examples of host-centric logs are:

- A user accessing a file
- A user attempting to authenticate.
A process Execution Activity
A process adding/editing/deleting a registry key or value.
Powershell execution

# 2) Network-Centric Log Sources

Network-related logs are generated when the hosts communicate with each other or access the internet to visit a website. Some network-based protocols are SSH, VPN, HTTP/s, FTP, etc. Examples of such events are:

SSH connection
A file being accessed via FTP
Web traffic
A user accessing company's resources through VPN.
Network file sharing Activity


