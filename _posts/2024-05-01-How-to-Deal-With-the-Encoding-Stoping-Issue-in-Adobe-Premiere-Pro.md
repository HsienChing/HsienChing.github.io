---
title: "[問題處理] 使用Adobe Premiere Pro輸出長影片，有好幾次編碼到92%時就Premiere就固定會停掉，如何處理?"
date: 2024-05-01 13:00:00 +0800
excerpt: "在測試使用Adobe Premiere Pro輸出長影片(片長約45分鐘)時，有好幾次編碼到92%時就Premiere就固定會停掉(機率80%左右)，有時是開始編碼就停掉(機率10%左右)，有時是編到40%左右就停掉(機率10%左右)。只能用工作管理員結束工作。該如何處理?"
categories: 
  - Problem
  - 問題
tags:
  - Solving issues
  - 處理問題
  - Adobe
  - Premiere
  - Nvidia
  - GPU
# toc: true
# toc_sticky: false
# toc_label: "Table of contents"
# toc_icon: "columns"
---

2024-04-10 星期三

在測試使用Adobe Premiere Pro輸出長影片(片長約45分鐘)時，有好幾次編碼到92%時就Premiere就固定會停掉(機率80%左右)，有時是開始編碼就停掉(機率10%左右)，有時是編到40%左右就停掉(機率10%左右)。只能用工作管理員結束工作。也搞不清楚怎麼回事。

只好來測試哪個方法可以解決:  
1. 電腦重新開機，無效。
2. 用舊版的影片，拉進Premiere，重新編輯，無效。(試到這裡就太晚了，先去睡覺。明天繼續測試。)
3. 將Premiere的工作資料夾，移動到D槽表層，並使用全英文資料夾名稱。無效。
4. 更新Nvidia RTX 3070Ti顯示卡驅動程式，無效。

暫時沒猜到問題，休息一下。

之後，又重複幾次編碼的動作，還是到92%時就停掉。

但這幾次編碼時，有注意到工作管理員的GPU使用狀況。竟然是由Intel的內建GPU在編碼，這很奇怪。怎麼不用Nvidia的GPU來編碼?

看起來這裡可能有問題。

於是去`File > Project Settings > General`，檢查`Video Rendering and Playback`中`Renderer`的選項。

發現竟然是選擇`Mercury Playback Engine GPU Acceleration (OpenCL)`，而不是選擇`Mercury Playback Engine GPU Acceleration (CUDA)`。

由於本台電腦使用Nvidia RTX 3070Ti顯示卡，當然是要選擇`Mercury Playback Engine GPU Acceleration (CUDA)`，這樣才能好好使用GPU的硬體加速。

把選項更改為`Mercury Playback Engine GPU Acceleration (CUDA)`後，編碼變快了，而且也一路順暢到100%，順利把影片編碼出來。

看起來這就是問題了! 運氣不錯，找到關鍵點! 成功解決!

PS 1: 原本還考慮過，是否是因為使用不同版本的Premiere才造成這個問題，但看起來並不是。

PS 2: 沒事不要隨便更新Premiere版本。

# 相關連結

[Mercury Playback Engine (GPU Accelerated) 演算器](<https://helpx.adobe.com/tw/x-productkb/multi/gpu-acceleration-and-hardware-encoding.html>)
