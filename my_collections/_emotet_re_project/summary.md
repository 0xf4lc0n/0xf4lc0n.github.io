---
layout: default
---

### Summary

I managed to reach the end of this project. Malware delivery chain was broken by domains' takedown and I wasn't able to analyse the exe content (And I think it's good because probably it would defeat me, so I definitely need more practise on simpler examples).

For example this looks quite scary ([Source](https://www.cert.pl/en/news/single/whats-up-emotet/)):
![](../assets/images/emotet_re_project/emotet_main.png)

I successfully deobfuscated macro's and dropper's code and the process of doing this was quite nice.

I learned about call operators in PowerShell ('.' and '&') and little bit about Win32_Process API. 
I had a chance to play with some new tools like oledump and gained some knowledge about creating PowerShell process by macro.

To sum up whole, the project was an interesting experience but quite too ambitious for my current skills. I think that more could be done in case of the domain analysis but deadline has come.

#### Table of content:

1.  [Environment configuration](environment-configuration.html)
2.  [Initial analysis](initial-analysis.html)
3.  [Macro analysis](macro-analysis.html)
4.  [Dropper analysis](dropper-analysis.html)
5.  [Detonation](detonation.html)
7.  [Domain analysis](domain-analysis.html)
8.  [Detonation ver. 2](detonation-v2.html)
9.  [Summary](summary.html)