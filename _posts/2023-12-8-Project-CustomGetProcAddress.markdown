---
layout: post
title:  "Custom GetProcAddress And GetModuleHandle"
date:   2023-12-8 9:50:00 +0530
category: projects
---

# **Description**

This project aims to provide a custom implementation of commonly used Windows API functions GetProcAddress() and GetModuleHandle() in Malware Development for Red Teaming/Offensive Security.

The GetModuleHandle() API is used in windows to retrieve a handle/pointer to a modules loaded in memory of a process at runtime. The usage of the API combined with GetProcAddress() can alert AV/EDR of the payload intentions and may be categorized as malicious.

Therefore, the custom implementation of GetModuleHandle() parses the different data structures like PEB, LDR_DATA_TABLE_ENTRY to find the base address of the DLL at runtime. This address is then passed to the custom implementation of GetProcAddress() which parses the PE structure and searches for the API address inside the IMAGE_EXPORT_DIRECTORY.

This avoids using the commonly known APIs anc can bring down the detection of a payload by AV/EDRs

# **Showcase**

![Image1](/files/images/Project-GetProcAddress/pic_GetDllBase.png)
![Image2](/files/images/Project-GetProcAddress/pic_GetProcAddress.png)

# **Video**
<video controls width="640" height="360">
  <source src="/files/Videos/Red Teaming Adversary Simulation Bypassing WD .mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>


# **Project Page**
- [https://github.com/hackerman008/Custom-GetProcAddress-GetModuleHandle](https://github.com/hackerman008/Custom-GetProcAddress-GetModuleHandle)