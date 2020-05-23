---
layout: default
---

WebClient from dropper module tried to download content from that sites:

```plain
http://www.pieceofpassion.net/0xrnl3/a27xm99fgd_on7xp-31134189/
http://www.marketfxelite.com/wp-admin/unnJtCHk/
https://tananfood.com/wp-includes/yoclwyWE/
http://raisabook.com/wp-content/NjBtuxBzkD/
http://biswalfoodcircle.com/vcobhlons/kaf6j_71wzkgvqso-8/
```

But during the second detonation it turned out that under presented URIs there was no content. Servers responded with 404 code.
I used URLhaus and I found out that all sites were taken down.

![](../assets/images/emotet_re_project/pieceofpassion_takedown.png)
![](../assets/images/emotet_re_project/marketfxelite_takedown.png)
![](../assets/images/emotet_re_project/tananfood_takedown.png)
![](../assets/images/emotet_re_project/raisabook_takedown.png)
![](../assets/images/emotet_re_project/biswalfoodcircle_takedown.png)

I also used Wayback Machine to check if I would be able to restore content of these sites. Interestingly enough, certain parts of each site weren't archived.

![](../assets/images/emotet_re_project/pieceofpassion_archived.png)
![](../assets/images/emotet_re_project/marketfxelite_archived.png)
![](../assets/images/emotet_re_project/tananfood_archived.png)
![](../assets/images/emotet_re_project/raisabook_archived.png)
![](../assets/images/emotet_re_project/biswalfoodcircle_archived.png)

At this point it became obvious that infrastructure is "dead". I searched the web for information about these domains and URIs and most of results indicated a link to the emotet.  

![](../assets/images/emotet_re_project/domains_pastedump.png)

Examples: [click](https://paste.cryptolaemus.com/emotet/2019/10/03/emotet-malware-IoCs_10-03-19.html), [click](https://gist.github.com/wagonza/eb37bc2f23afee07872b0260966cbbe3) and [click](https://www.joesandbox.com/analysis/189358/0/executive).

#### Table of content:

1.  [Environment configuration](environment-configuration.html)
2.  [Initial analysis](initial-analysis.html)
3.  [Macro analysis](macro-analysis.html)
4.  [Dropper analysis](dropper-analysis.html)
5.  [Detonation](detonation.html)
7.  [Domain analysis](domain-analysis.html)
8.  [Detonation ver. 2](detonation-v2.html)
9.  [Summary](summary.html)