---
layout: default
---

### Detonation v2

Analysis of the domains showed that web infrastructure was out-of-date. In order to confirm that I tried to download content of 126.exe file.
To do that I connected my Windows machine to the Internet and I installed and turned on Wireshark on it.

I started PowerShell with administrator's privileges and I allowed for PowerShell scripts execution:

```powershell                
Set-ExecutionPolicy RemoteSigned
```   
I opened PowerShell ISE and pasted there the dropper content and saved it to a .ps1 file.

![](../assets/images/emotet_re_project/ps_ise.png)

I added comment to line 17.

![](../assets/images/emotet_re_project/commented_ise.png)

And finally started the script. When script ended execution I opened Wireshark, stopped the capturing and used http filter.
I saw that 3 domains responded with 404 code.

![](../assets/images/emotet_re_project/404.png)
![](../assets/images/emotet_re_project/pieceofpassion.png)
![](../assets/images/emotet_re_project/marketfxelite.png)
![](../assets/images/emotet_re_project/raisabook.png)

Traffic to tananfood.com was encrypted:

![](../assets/images/emotet_re_project/tanafood.png)

And traffic for biswalfoodcircle.com looked like this:

![](../assets/images/emotet_re_project/biswalfoodcircle.png)

When I tried to open that site in browser, I got similar results. Site was loading very long and provided no content.

#### Table of content:

1.  [Environment configuration](environment-configuration.html)
2.  [Initial analysis](initial-analysis.html)
3.  [Macro analysis](macro-analysis.html)
4.  [Dropper analysis](dropper-analysis.html)
5.  [Detonation](detonation.html)
7.  [Domain analysis](domain-analysis.html)
8.  [Detonation ver. 2](detonation-v2.html)
9.  [Summary](summary.html)