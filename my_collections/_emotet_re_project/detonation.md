---
layout: default
---

### Detonation

Here I performed controlled execution. For that I userd Wireshark, Burp Suite, INetSim and Procmon.

I opened malicious doc file and turned on macros.

![](../assets/images/emotet_re_project/procmon.png)

After a while I turned off capturing in Procmon (Ctrl + E) and then I opened Process Tree (Ctrl + T). I expanded wininit.exe and searched PowerShell process started by wmiprvse.exe.

![](../assets/images/emotet_re_project/wininit_exe.png)
![](../assets/images/emotet_re_project/power_shell.png)

Then I right clicked on PowerShell process and selected option "Go To Event". I closed Process Tree and in Procmon I right clicked on PowerShell entry and selected Properties.

![](../assets/images/emotet_re_project/power_shell_proc_mon.png)

In "Event" and "Process" tab I found encoded PowerShell script, but I had already got it. Then I went to home directory and found there 126.exe.

![](../assets/images/emotet_re_project/126_exe.png)

The file should contain html code from INetSim and it really did.

![](../assets/images/emotet_re_project/hxd.png)

On my Linux machine I turned off INetSim and opened report file. The most interesting was the following part:

```plain
2020-01-12 18:53:52  DNS connection, type: A, class: IN, requested name: www.pieceofpassion.net
2020-01-12 18:53:52  HTTP connection, method: GET, URL: http://www.pieceofpassion.net/0xrnl3/a27xm99fgd_on7xp-31134189/, file name: data/http/fakefiles/sample.html
2020-01-12 18:53:52  DNS connection, type: A, class: IN, requested name: www.marketfxelite.com
2020-01-12 18:53:52  HTTP connection, method: GET, URL: http://www.marketfxelite.com/wp-admin/unnJtCHk/, file name: data/http/fakefiles/sample.html
2020-01-12 18:53:52  DNS connection, type: A, class: IN, requested name: tananfood.com
2020-01-12 18:53:53  DNS connection, type: A, class: IN, requested name: raisabook.com
2020-01-12 18:53:53  HTTP connection, method: GET, URL: http://raisabook.com/wp-content/NjBtuxBzkD/, file name: data/http/fakefiles/sample.html
2020-01-12 18:53:53  DNS connection, type: A, class: IN, requested name: biswalfoodcircle.com
2020-01-12 18:53:53  DNS connection, type: A, class: IN, requested name: biswalfoodcircle.com
2020-01-12 18:53:53  HTTP connection, method: GET, URL: http://biswalfoodcircle.com/vcobhlons/kaf6j_71wzkgvqso-8/, file name: data/http/fakefiles/sample.html
```
                
Additionaly Burp Suite captured that:

![](../assets/images/emotet_re_project/burp_suite_https_con.png)

So domains that I had to check were:

```plain   
http://www.pieceofpassion.net/0xrnl3/a27xm99fgd_on7xp-31134189/
http://www.marketfxelite.com/wp-admin/unnJtCHk/
http://raisabook.com/wp-content/NjBtuxBzkD/
http://biswalfoodcircle.com/vcobhlons/kaf6j_71wzkgvqso-8/
https://tananfood.com/wp-includes/yoclwyWE/
```             
I used "http || udp" filter to gain information about packet traffic:

![](../assets/images/emotet_re_project/wireshark_inetsim.png)

#### Table of content:

1.  [Environment configuration](environment-configuration.html)
2.  [Initial analysis](initial-analysis.html)
3.  [Macro analysis](macro-analysis.html)
4.  [Dropper analysis](dropper-analysis.html)
5.  [Detonation](detonation.html)
7.  [Domain analysis](domain-analysis.html)
8.  [Detonation ver. 2](detonation-v2.html)
9.  [Summary](summary.html)