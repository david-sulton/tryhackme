# Windows PowerShell
- Easy room
- https://tryhackme.com/room/windowspowershell
## What Is PowerShell
- From the official Microsoft [page](https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.4): “PowerShell is a cross-platform task automation solution made up of a command-line shell, a scripting language, and a configuration management framework.”
## Powershell Basics
- ```Get-Content```: Retrieves (gets) the content of a file and displays it in the console.
- ```Set-Location```: Changes (sets) the current working directory.
- ```Get-Command``` It’s an essential tool for discovering what commands one can use.
- ```Get-Help```: it provides detailed information about cmdlets, including usage, parameters, and examples
- By appending ```-examples``` to the command displayed above, we will be shown a list of common ways in which the chosen cmdlet can be used.
- To search for modules (collections of cmdlets) in online repositories like the PowerShell Gallery, we can use ```Find-Module```
- Once identified, the modules can be downloaded and installed from the repository with ```Install-Module```, making new cmdlets contained in the module available for use.
## Navigating the File System and Working with Files
- ```Get-ChildItem```  lists the files and directories in a location specified with the -Path parameter
- ```Set-Location``` It changes the current directory, bringing us to the specified path, akin to the cd command in Command Prompt
- To create an item in PowerShell, we can use ```New-Item```. We will need to specify the path of the item and its type (whether it is a file or a directory)
- ```Remove-Item``` cmdlet removes both directories and files, whereas in Windows CLI we have separate commands ```rmdir``` and ```del```
- ```Copy-Item``` (equivalent to copy) and ```Move-Item``` (equivalent to move)
- ```Get-Content``` cmdlet, which works similarly to the type command in Command Prompt (or cat in Unix-like systems).
## Piping, Filtering, and Sorting Data
### Examples
-  ```Get-ChildItem | Sort-Object Length```
-  ```Get-ChildItem | Where-Object -Property "Extension" -eq ".txt"```
-  ```Get-ChildItem | Where-Object -Property "Name" -like "ship*"```
-  ```Get-ChildItem | Select-Object Name,Length```
-  ```Select-String -Path ".\captain-hat.txt" -Pattern "hat"```
-  ```Get-ChildItem | Where-Object -Property Length -gt 100```
## System and Network Information
- ```Get-ComputerInfo``` cmdlet retrieves comprehensive system information, including operating system information, hardware specifications, BIOS details, and more.
- ```Get-LocalUser```
- ```Get-NetIPConfiguration``` provides detailed information about the network interfaces on the system, including IP addresses, DNS servers, and gateway configurations.
- ```Get-NetIPAddress``` cmdlet will show details for all IP addresses configured on the system, including those that are not currently active.
## Real-Time System Analysis
- ```Get-Process``` provides a detailed view of all currently running processes, including CPU and memory usage, making it a powerful tool for monitoring and troubleshooting.
- ```Get-Service``` allows the retrieval of information about the status of services on the machine, such as which services are running, stopped, or paused. It is used extensively in troubleshooting by system administrators, but also by forensics analysts hunting for anomalous services installed on the system.
- ```Get-NetTCPConnection``` displays current TCP connections, giving insights into both local and remote endpoints.
- ```Get-FileHash``` as a useful cmdlet for generating file hashes, which is particularly valuable in incident response, threat hunting, and malware analysis, as it helps verify file integrity and detect potential tampering.
## Scripting
- Scripting is the process of writing and executing a series of commands contained in a text file, known as a script, to automate tasks that one would generally perform manually in a shell, like PowerShell.
- Simply speaking, scripting is like giving a computer a to-do list, where each line in the script is a task that the computer will carry out automatically. This saves time, reduces the chance of errors, and allows to perform tasks that are too complex or tedious to do manually. As you learn more about shells and scripting, you’ll discover that scripts can be powerful tools for managing systems, processing data, and much more.
- ```Get-Help Invoke-Command -examples```
- ```Invoke-Command -ComputerName RoyalFortune -ScriptBlock {Get-Service}```
## Conclusion
