---
layout: page
title: About
permalink: /about/
---

I am a self-taught Malware Analyst and Red Team enthusiast with practical experience in analyzing complex malware and building red teaming tools. My journey in cybersecurity started two years ago and from that time I have been constantly gaining new skills and honing my craft in both defense and offensive security. 

<!-->
### Offensive Security

**Custom command cnd control infrastructure for post-exploitaion activity**
- [Demo1](https://youtu.be/OCv5dNi5JvE) - The demo demonstrates capability to bypass Comodo Next Generation Antivirus
- [Demo2](https://youtu.be/AEn55czPIbo) - The demo demonstrates capability to bypass Windows Defender Cloud enabled protection
- [Demo3](https://youtu.be/cIRFxURciwY) - The demo showcases the runtime AV testing capability of the implant using custom process injection commands.
- **Features implemented**
    - Reflective loading for implant
    - Directory traversal and listing files
    - Downloading and uploading files
    - Retrieving DLLs loaded in process memory
    - Command for process injection
    - Runtime decryption of uploaded payload to prevent detection using memory scanning
    - Command to detect API hooks implemented by AV/EDR
    - Unhook command to remove ntdll API hooks implemented by AV/EDR
    - Command to execute third stage payloads locally or in remote process
    - COmmand to retrieve syscall ID dynamically
    - Ability to use win32, native APIs or direct syscalls for commands at runtime
    - Custom GetProcAddress and LoadLibrary API implementation
    - Encryption of specific code section to avoid static detection of assembly instruction before implant execution

[Github](https://github.com/prakashyadav008/Offensive-Security/blob/main/Malware%20Development%20For%20Offsec/Writeups/Implementing%20Custom%20Malware%20Loaders%201/Implementing%20Custom%20Malware%20Loaders%201.pdf) **Implementing custom malware loader** 
- This write-up goes into how to implement a 2-stage malware consisting of a custom stage1 loader to deploy a stage2 payload
- Covers payload storage in resource section and preventing payload detection by Anti-virus

[Github](https://github.com/prakashyadav008/Offensive-Security/blob/main/Malware%20Development%20For%20Offsec/Writeups/Retrieving%20native%20api%20address%20and%20syscall%20id%20at%20runtime/Retrieving_ntdll_base_address_syscallid.pdf) **Retrieving native API address and syscall IDs at runtime** 
- This write-up goes into the details of retrieving native API addresses at runtime which could be used for direct syscalls effective in bypassing API hooks placed by AV/EDR soulutions in win32 APIs in user mode processes.

### Malware Analysis

[Github](https://github.com/prakashyadav008/Insight-into-malware/blob/main/Malware_analysis6.pdf) **Bumblebee malware analysis**

[Github](https://github.com/prakashyadav008/Insight-into-malware/blob/main/malware_analysis5.pdf) **Emotet malware analysis**

[Github](https://github.com/prakashyadav008/Insight-into-malware/blob/main/malware_analysis4.pdf) **Guloader malware analysis**

[Github](https://github.com/prakashyadav008/Insight-into-malware/blob/main/malware_anaysis3.pdf) **Trickbot loader malware analysis**
