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

# Logs on Windows (Event Viewer)
![image](https://github.com/user-attachments/assets/a7d34232-8be0-4bf5-806c-8757687df79a)

# Logs on Linux
Linux Workstation

Linux OS stores all the related logs, such as events, errors, warnings, etc. Which are then ingested into SIEM for continuous monitoring. Some of the common locations where Linux store logs are:

- /var/log/httpd : Contains HTTP Request  / Response and error logs.
- /var/log/cron   : Events related to cron jobs are stored in this location.
- /var/log/auth.log and /var/log/secure : Stores authentication related logs.
- /var/log/kern : This file stores kernel related events.
```journalct -f``` shows all logs for linux system in real time (following)

![image](https://github.com/user-attachments/assets/0e6d38aa-bae5-4dc0-944e-8db61a2d1731)


# SIEM is just one part of the SOC
![image](https://github.com/user-attachments/assets/02e57dcb-f169-4c63-b13d-88898d5d36a0)

# Dashboard

- Dashboards are the most important components of any SIEM. SIEM presents the data for analysis after being normalized and ingested. The summary of these analyses is presented in the form of actionable insights with the help of multiple dashboards. Each SIEM solution comes with some default dashboards and provides an option for custom Dashboard creation. Some of the information that can be found in a dashboard are:

- Alert Highlights
- System Notification
- Health Alert
- List of Failed Login Attempts
- Events Ingested Count
- Rules triggered
- Top Domains Visited

An example of a Default dashboard in Qradar SIEM is shown below:
![image](https://github.com/user-attachments/assets/59afc93d-99f3-4354-b705-9279fe63baab)

# Correlation Rules

- Correlation rules play an important role in the timely detection of threats allowing analysts to take action on time. Correlation rules are pretty much logical expressions set to be triggered. A few examples of correlation rules are:
```
If a User gets 5 failed Login Attempts in 10 seconds - Raise an alert for Multiple Failed Login Attempts
If login is successful after multiple failed login attempts - Raise an alert for Successful Login After multiple Login Attempts
A rule is set to alert every time a user plugs in a USB (Useful if USB is restricted as per the company policy)
If outbound traffic is > 25 MB - Raise an alert to potential Data exfiltration Attempt (Usually, it depends on the company policy)
```



