---
title: "[Microsoft][Windows] 如何在多台電腦上，使用同一組滑鼠鍵盤，並且互傳檔案"
date: 2026-05-23 00:10:00 +0800
excerpt: "[Microsoft][Windows] 如何在多台電腦上，使用同一組滑鼠鍵盤，並且互傳檔案"
categories: 
  - Study
  - 學習
tags:
  - Microsoft
  - Windows
  - Multi-computer
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

當手上電腦有很多台後，要在這些電腦間切換鍵盤滑鼠進行工作，就變得麻煩。

例如: 當有兩台電腦時，桌上就有兩組鍵盤滑鼠。換來換去很麻煩，有沒有辦法共用同一組鍵盤滑鼠?

該如何處理?

# Microsoft Windows 系統的解決方案

使用 Microsoft 出的 [Microsoft Garage Mouse without Borders](https://www.microsoft.com/en-us/download/details.aspx?id=35460) 即可解決。

該軟體可以最多連接4台安裝 Microsoft Windows 系統的電腦，並可以實現任意電腦的鍵盤、滑鼠共用 (或混用)，並且支援檔案傳輸。

PS: 要注意，要在同一個區域網路下才行。

例如，原來兩台電腦都在跟 Wi-Fi A 連線，這樣兩台電腦就可以共用鍵盤滑鼠，互相傳輸檔案。但突然間，其中一台電腦跑去跟 Wi-Fi B 連線，這樣兩台電腦就 "斷路" 了，沒辦法共用鍵盤滑鼠，也不能互相傳輸檔案了。

# 安裝方式

在想要操作的電腦中，都安裝該軟體，並記下 `Security key` 及 `Computer's name`。

例如: 在 A 電腦中，記下 A 電腦的 `Security key` 及 `Computer's name`，再到 B 電腦中的 `Microsoft Garage Mouse without Borders` 輸入 A 電腦的 `Security key` 及 `Computer's name`，再按 LINK，連線即可。

> Mouse without Borders (http://aka.ms/mm) is a product that makes you the captain of your computer fleet by allowing you to control up to four computers from a single mouse and keyboard. This means that with Mouse without Borders you can copy text or drag and drop files across computers.

> **Install Instructions**
> Download and run the installer on each of your machines. The Mouse without Borders setup experience will be launched after installation. Follow the instructions to configure Mouse without Borders. NOTE: The same version of Mouse without Borders must be run in the machines, old version can be uninstalled in Control Panel or by just running this command: msiexec /uninstall {D3BC954F-D661-474C-B367-30EB6E56542E} /qr. Visit http://aka.ms/mm for help & questions.

# 使用限制

1. 需安裝 Microsoft Windows 作業系統的電腦。
2. 每台電腦都要安裝 `Microsoft Garage Mouse without Borders` 軟體。
3. 每台電腦都要在同一個區域網路下。
4. 如果有網路斷線狀況發生，可能需要手動重新啟動 `Microsoft Garage Mouse without Borders` 軟體，再度重新 `LINK`。通常等個幾秒鐘就會自動回復。
5. 由於是使用網路傳輸不同電腦間的鍵盤滑鼠資料，因此會受到網路速度的影響，可能會有卡頓的狀況發生。例如: 使用 Wi-Fi 建立的區域網路，使用起來就會較為卡頓，會看到滑鼠跳來跳去，移動並不流暢。
6. 建議使用同一台電腦上的鍵盤滑鼠，去其他台電腦進行操作。避免一些意外狀況。例如: 可能會發生使用 B 電腦的滑鼠，在 B 電腦的畫面後，無法使用 A 電腦的鍵盤進行打字。
7. 該軟體的主要判斷方式為，用哪台電腦的滑鼠，就會用哪台電腦的鍵盤當作主要鍵盤。

# 長期使用下的感受

以下是經過數周使用後的感受 (記錄日期: 2026-06-17)

1. 主要還是卡頓感。使用 Wi-Fi 建立的區域網路，使用起來就會較為卡頓，會看到滑鼠跳來跳去，移動並不流暢。如果要將滑鼠精準定位，滑鼠可能會跳來跳去，很難道目標位置，就會很煩。而鍵盤也會發生類似的事情，主要是在鍵盤上已經打字了，但螢幕上出現字的時間有點延遲。一開始還會以為是自己打錯了，還會去再重新打字，後來才發現是延遲。
2. 而且儘管已經在一個區域網路下，也會常常找不到另一台電腦。就要去另一台電腦，手動重啟 `Mouse without Borders` 軟體。頻率大概是每天發生幾次。
3. 兩台電腦間會互相干擾。例如:  
    案例1: 如果另一台電腦A在打遊戲進入全螢幕模式，這時候滑鼠移動到另一台電腦B上時，電腦A上的全螢幕遊戲畫面就會閃一下，跳出全螢幕模式。  
    案例2: 在電腦A的滑鼠鍵盤動作操作正常，但到電腦B時，滑鼠鍵盤動作操作就突然變得不正常。例如: 滑鼠滾輪在電腦A的功能是網頁頁面上下滑動，正常情況下，在電腦B也會是同樣功能，但有時候在電腦B時，滾輪功能會變成網頁頁面縮放。鍵盤也有類似狀況，在電腦A的功能是正常打字，在電腦B同樣的按鍵突然變成功能快捷鍵，無法打字。
4. 大致上，一般文書使用上還可以接受。
5. 若使用有線 (RJ-45網路線) 的區域網路，可能卡頓感就會降低，但目前還沒有去嚐試。

# 其他的類似軟體

羅技的 Flow 技術，搭配自家的特定專用鍵盤滑鼠，也可以達到相同功能。但在設定時，也是要在同一個區域網路下，看來就算用羅技的生態系，如果又是在 Wi-Fi 的建立的區域網路下，仍然會遭遇相同的卡頓狀況。

# 參考資料

1. [Microsoft Garage Mouse without Borders](https://www.microsoft.com/en-us/download/details.aspx?id=35460)
2. [羅技的 Flow 技術](https://www.logitech.com/zh-tw/software/flow)
