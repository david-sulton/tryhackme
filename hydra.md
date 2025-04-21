# Hydra
![image](https://github.com/user-attachments/assets/5d0d88ed-7793-4a31-be12-f3f48999d212)

## What is Hydra
Hydra is a brute force online password cracking program, a quick system login password “hacking” tool.

Hydra can run through a list and “brute force” some authentication services. Imagine trying to manually guess someone’s password on a particular service (SSH, Web Application Form, FTP or SNMP) - we can use Hydra to run through a password list and speed this process up for us, determining the correct password.

According to its official repository, Hydra supports, i.e., has the ability to brute force the following protocols: “Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC and XMPP.”

For more information on the options of each protocol in Hydra, you can check the Kali Hydra tool page.

This shows the importance of using a strong password; if your password is common, doesn’t contain special characters and is not above eight characters, it will be prone to be guessed. A one-hundred-million-password list contains common passwords, so when an out-of-the-box application uses an easy password to log in, change it from the default! CCTV cameras and web frameworks often use admin:password as the default login credentials, which is obviously not strong enough.

## Installing Hydra
    apt install hydra
    dnf install hydra

  
## Hydra Commands
The options we pass into Hydra depend on which service (protocol) we’re attacking. For example, if we wanted to brute force FTP with the username being user and a password list being passlist.txt, we’d use the following command:

      hydra -l user -P passlist.txt ftp://MACHINE_IP

  ![image](https://github.com/user-attachments/assets/b2dbc05e-049f-4de4-9f6b-0f326808774e)

    hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V

    The login page is only /, i.e., the main IP address.
    The username is the form field where the username is entered
    The specified username(s) will replace ^USER^
    The password is the form field where the password is entered
    The provided passwords will be replacing ^PASS^
    Finally, F=incorrect is a string that appears in the server reply when the login fails

  
