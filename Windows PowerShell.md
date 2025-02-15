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
