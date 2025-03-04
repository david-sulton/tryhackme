![image](https://github.com/user-attachments/assets/af13983b-f2eb-4e17-8104-32682ffa81ef)

# Nmap: The Basics
- https://tryhackme.com/room/nmap 
## Host Discovery: Who is Online
```nmap -sn 1.1.1.1/24```
```nmap -sL 1.1.1.1/24```
## Port Scanning: Who is Listening
```nmap -sT 1.1.1.1/24```
```nmap -sS 1.1.1.1/24```
```nmap -sU 1.1.1.1/24```

## Version Detection: Extract More Information
```nmap -sS -O 1.1.1.1/24``` OS detection
```nmap -sS -sV 1.1.1.1/24``` service and version detection
## Timing: How fast is fast
![image](https://github.com/user-attachments/assets/fbcfca97-dbca-43c4-8190-ea29b258c611)

## Output: Controlling what you see
Saving Scan Report

- In many cases, we would need to save the scan results. Nmap gives us various formats. The three most useful are normal (human-friendly) output, XML output, and grepable output, in reference to the grep command. You can select the scan report format as follows:

      -oN <filename> - Normal output
      -oX <filename> - XML output
      -oG <filename> - grep-able output (useful for grep and awk)
      -oA <basename> - Output in all major formats

- In the terminal below, we can see an example of using the -oA option. It resulted in three reports with the extensions nmap, xml, and gnmap for normal, XML, and grep-able output.
## Conclusion
![image](https://github.com/user-attachments/assets/7de6f246-858c-4970-a740-ed611d2acffe)
