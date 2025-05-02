# CAPA: The Basics

![image](https://github.com/user-attachments/assets/09be62bd-cc3e-4b90-890c-ed2db148be40)

- CAPA (Common Analysis Platform for Artifacts) is a tool developed by the FireEye Mandiant team. It is designed to identify the capabilities present in executable files like Portable Executables (PE), ELF binaries, .NET modules, shellcode, and even sandbox reports. It does so by analyzing the file and applying a set of rules that describe common behaviours, allowing it to determine what the program is capable of doing, such as network communication, file manipulation, process injection, and many more.

- The beauty of CAPA is that it encapsulates years of reverse engineering knowledge into an automated tool, making it accessible even to those who may not be experts in reverse engineering. This can be incredibly useful for analysts and security professionals, allowing them to quickly understand potentially malicious software's functionality without manually reverse engineering the code.

- This tool is particularly useful in malware analysis and threat hunting, where understanding a binary's capabilities is crucial for incident response and defensive measures.

      capa.exe <.\binary file>
  ![image](https://github.com/user-attachments/assets/b1b6933b-697f-4d2d-ba3b-74b5561ee395)


![image](https://github.com/user-attachments/assets/501406e7-9481-45c8-a036-4aa5b796b630)


