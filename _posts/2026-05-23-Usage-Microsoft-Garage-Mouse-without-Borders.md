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
4. 如果有網路斷線狀況發生，可能需要手動重新啟動 `Microsoft Garage Mouse without Borders` 軟體，再度重新 `LINK`。
5. 由於是使用網路傳輸不同電腦間的鍵盤滑鼠資料，因此會受到網路速度的影響，可能會有卡頓的狀況發生。
6. 建議使用同一台電腦上的鍵盤滑鼠，去其他台電腦進行操作。避免一些意外狀況。例如: 可能會發生使用 B 電腦的滑鼠，在 B 電腦的畫面後，無法使用 A 電腦的鍵盤進行打字。
7. 該軟體的主要判斷方式為，用哪台電腦的滑鼠，就會用哪台電腦的鍵盤當作主要鍵盤。

# 參考資料
[Microsoft Garage Mouse without Borders](https://www.microsoft.com/en-us/download/details.aspx?id=35460)
