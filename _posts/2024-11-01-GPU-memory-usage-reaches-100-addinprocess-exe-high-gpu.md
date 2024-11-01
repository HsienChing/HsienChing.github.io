---
title: "[電腦維修][掃毒][木馬] GPU記憶體使用率達到100% / addinprocess.exe high gpu / addinprocess.exe virus <br> [Computer Repair][Anti-Virus][Trojan] GPU memory usage reaches 100% / addinprocess.exe high gpu / addinprocess.exe virus"
date: 2024-06-13 00:11:00 +0800
excerpt: "紀錄處理木馬的過程。後來發現是木馬造成GPU記憶體使用率達到100%。 <br> Record the process of handling Trojans. It was later discovered that a Trojan caused the GPU memory usage to reach 100%."
categories:
  - Solving issues
  - 處理問題
tags:
  - Computer
  - 電腦
  - Antivirus
  - 掃毒
  - Trojan
  - 木馬
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

# 狀況

2024-10-31 星期四

筆電這幾天待機時，風扇會一陣陣地轉，聲音很吵。怪怪的。而且筆電也會變得很熱。

去看工作管理員，發現就算完全不執行作業，光放著，GPU就會有間歇性資源使用比率衝到100%的狀況。

但是把網路關掉後，就沒有這狀況了。

直覺告訴我，筆電裡面有鬼。

# 處理方式

## 使用Windows Defender的快速掃描 (抓到2隻木馬)

打開Windows Defender，開啟快速掃描，發現2隻木馬。

BINGO!

殺這兩隻後，仍然存在該問題。

## 使用Windows Defender的完整掃描 (抓到5隻木馬)

使用完整掃描，掃描了2小時後，預估完成時間越變越長，於是停止掃描。雖然還沒完全掃完，但發現5隻木馬。

殺殺殺殺殺!

## 使用Windows Defender的自訂掃描

使用自訂掃描，掃描整個C槽，沒全部掃完，預估完成時間越變越長，於是停止掃描。Windows Defender沒有發現問題。但筆電仍然運作不正常。

## 使用Windows Defender的離線掃描

使用離線掃描，Windows Defender說沒有發現問題。但筆電仍然運作不正常。

## 使用工作管理員觀察電腦運作，發現問題

2024-11-01 星期五

從工作管理員上發現，是"addinprocess.exe"這個程式造成GPU使用率達到100%的問題，並且也造成GPU記憶體使用率100%。

把這個程式名稱打到google上，就會看到一些關鍵字，看來很多人也發生類似的問題  
關鍵字1: addinprocess.exe high gpu  
關鍵字2: addinprocess.exe virus

1. [High GPU Usage thanks to "AddinProcess.exe"](https://answers.microsoft.com/en-us/windows/forum/all/high-gpu-usage-thanks-to-addinprocessexe/3a0ec46e-7f2f-44fe-b277-95fcaad1c090)
2. [AddInProcess.exe 100% Gpu usage, why? and how to fix it??](https://answers.microsoft.com/en-us/windows/forum/all/addinprocessexe-100-gpu-usage-why-and-how-to-fix/4f8890d1-8169-4cbd-9023-043d05523d45)
3. [AddInProcess.exe is using my gpu at 100%, can't delete it.](https://www.reddit.com/r/techsupport/comments/ynsuen/addinprocessexe_is_using_my_gpu_at_100_cant/)
4. [AddInprocess.exe is hogging GPU memory and making games runs choppy](https://forums.tomshardware.com/threads/addinprocess-exe-is-hogging-gpu-memory-and-making-games-runs-choppy.3713535/)
5. [Addinprocess.exe: What Is It & How to Remove It](https://windowsreport.com/addinprocess-exe/)
6. [AddInProcess.exe](https://answers.microsoft.com/en-us/windows/forum/all/addinprocessexe/89c139f6-28ad-45bb-a41c-d07343548856)

## 猜答案

如果要全部看完，那我看也下課了!

所以，要開始猜答案了!

## 認識"addinprocess.exe"

首先要認識什麼是"addinprocess.exe"  
"addinprocess.exe"這個程式是在這個資料夾中，表示他是Microsoft的程式，不是病毒。  
存放在這個資料夾中 "C:\Windows\Microsoft.NET\Framework64\v4.0.30319"

既然這個軟體是微軟的正常軟體，那麼兇手可能就是其他人了。

## 手動測試從工作管理員結束"addinprocess.exe"

手動從工作管理員結束"addinprocess.exe"這個程式的話，會看到GPU記憶體使用率由100%瞬間降到接近0%。

但高興不到幾秒鐘，使用率馬上又飆回100%。因為"addinprocess.exe"又重新啟動了。

但至少我們看到問題了。

## 尋求解決方案

網路上提供的解決方案有很多  
大致上有三招  
(前提要先更新Windows到最新，再來試這三招)  
1. 移除.NET\Framework再重新安裝
2. 找到觸發這個程式的病毒或相關軟體，並移除。
3. 使用"Unlocker"程式，來自動結束"addinprocess.exe"的自動啟動。

## 測試解決方案3，使用"Unlocker"程式。無效

使用第三招，使用"Unlocker"程式。

下載並安裝Unlocker

但該程式在下載及安裝時，被Windows Defender警告有問題。於是就不顧"安全性建議"，仍然安裝完成。
(Windows Defender回報會有 [PUA:Win32/BabylonToolbar](https://www.microsoft.com/en-us/wdsi/threats/malware-encyclopedia-description?name=PUA%3AWin32%2FBabylonToolbar&threatid=227720) 的問題)

安裝Unlocker後，使用Unlocker來自動結束"addinprocess.exe"的自動啟動。但GPU使用率仍處於100%。

重新開機。

開機後，網路此時是關閉，這時GPU記憶體使用率為接近0 (0.3/8.0 GB)。

開啟網路後，隨著網路流量進入，GPU記憶體使用率開始飆升，表示有外來因素觸發。

表示第三招，使用"Unlocker"程式。無效!

REF:  
Wikipedia: [Unlocker](https://zh.wikipedia.org/zh-tw/Unlocker)

Unlocker官網: <http://emptyloop.com/unlocker/>
但download的link進去後，卻看到"404 - File or directory not found."

第三方下載點: <https://unlocker.en.uptodown.com/windows>

## 測試解決方案2，找到觸發這個程式的病毒或相關軟體，並移除。成功!

使用第二招，找到觸發這個程式的病毒或相關軟體，並移除。

在工作管理員上面看有沒有異常的程式，看了10分鐘，沒感覺，放棄。

進去Windows Defender的"排除項目"，赫然發現有4個排除項目，其中一個就是"addinprocess.exe"

看來病毒已經"聰明"到會躲Windows Defender了。

"叫警察不要去他家查戶口!"

將這些排除項目，全部清空。

更新Windows後，重新開機。

開機後，網路此時是關閉，這時GPU記憶體使用率為接近0 (0.3/8.0 GB)。

開啟網路後，隨著網路流量進入，GPU記憶體使用率開始飆升，表示有外來因素觸發。

Windows Defender發現1隻木馬"Trojan:Win32/AgentTesla!ml"

儘管已經移除該木馬，但此時GPU記憶體使用率已經飆升至100% (8.0/8.0 GB)。

重新開機。

開機後，網路此時是關閉，這時GPU記憶體使用率為接近0 (0.1/8.0 GB)。

過了10分鐘 (13:11~13:21)，此時GPU記憶體使用率仍接近0 (0.1/8.0 GB)，看來已經處理掉問題了。

<figure>
<center>
  <img src="{{ site.baseurl }}{% link /assets/images/2024/Windows-Defender-Protection-History-2024-11-01.jpg %}" alt="" style="width:90%">
  <figcaption>Windows Defender的保護歷程記錄。可看到木馬名稱為"Trojan:Win32/AgentTesla!ml"，威脅等級屬於嚴重，受影響項目也很多，有幾個還是在排除項目的名單中。 (截圖時間: 2024-11-01)。</figcaption>
</center>
</figure>

## 長時間使用測試，OK!

開始用筆電進行平常的文書工作。經過小時測試，根據使用的軟體，GPU記憶體使用率會稍微提升 (0.5/8.0 GB)，但沒有暴衝的狀況發生了 (8.0/8.0 GB)，看來已經處理掉問題了。

# 感想

運氣不錯，在感冒的狀況下，還順利處理完畢!

可以好好休息一下了! 感恩!
