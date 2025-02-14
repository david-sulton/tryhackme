# Windows Command Line 
- https://tryhackme.com/room/windowscommandline
## Intro
- Easy room
- cmd.exe can be used for ssh
## Basic System Information
- ```set``` to check your path from the command line
- ```ver``` determine the operating system (OS) version
- ```systeminfo```command to list various information about the system such as OS information, system details, processor and memory
- ```driverquery | more``` show drivers
- ```help``` Provides help information for a specific command
- ```cls``` Clears the Command Prompt screen
## Network Troubleshooting
- ```ipconfig``` check network info
- ```ipconfig /all``` more info about network configuration
- ```ping google.com``` send ping ICMP packets and listen for response to google.com
- ```tracert google.com```traces the network route traversed to reach the target
- ```nslookup google.com```looks up a host or domain and returns its IP address
- ```netstat``` displays current network connection and listening ports
## Directories
- ```cd``` current directory or to change directory
- ```cd Users``` change directory to users
- ```cd ..``` go one level up in file system
- ```dir``` list child directories
- ```dir /a```Displays hidden and system files as well.
- ```dir /s```Displays files in the current directory and all subdirectories.
- ```tree``` to visually represent the child directories and subdirectories.
- ```mkdir``` make directory
- ```rmdir``` remove directory
- ```type``` view text files in command line
- ```more``` shows one page at a time
- ```copy``` copy files from one location to another
- ```move``` move files from one loacation to another
- ```del``` delete a file
- ```erase```delete a file
- ```*``` Can refer to multiple files
## Task and Process Management
- ```tasklist``` list running processes
      - Some filtering is helpful because the output is expected to be very long. You can check all available filters by displaying the help page using tasklist /?. Let’s say that we want to search for tasks related to sshd.exe, we can do that with the command ```tasklist /FI "imagename eq sshd.exe"```. Note that /FI is used to set the filter image name equals sshd.exe
-  ```taskkill /PID target_pid``` kill a process
## Conclusion
- ```chkdsk```checks the file system and disk volumes for errors and bad sectors.
- ```driverquery```displays a list of installed device drivers.
- ```sfc /scannow```scans system files for corruption and repairs them if possible.
- ```/?``` can be used with most commands to display a help page.
- Display text files: ```more file.txt```

