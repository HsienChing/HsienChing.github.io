---
title: "[AI] 如何讓 Stable Diffusion WebUI Forge 使用 RTX 5090 顯卡 (舊檔修理方式)"
date: 2026-05-24 00:10:00 +0800
excerpt: "最近剛入手一台筆電，具備 Nvidia GeForce RTX 5090 Laptop 顯卡， VRAM 有 24 GB，想說可以好好跑一下 Stable Diffusion WebUI Forge 的軟體了。就把原本在 RTX 3070 Ti (VRAM 8 GB) 筆電上跑的好好的 Stable Diffusion WebUI Forge (以下簡稱 `Forge`) 的軟體，整個資料夾，複製到新筆電上。想說就這樣直接使用看看。當然，事情沒那麼順利，直接出現一堆 error code。該如何解決?"
categories: 
  - Study
  - 學習
tags:
  - AI
  - Windows
  - StableDiffusion
  - WebUI
  - Forge
  - RTX5090
  - Nvidia
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

# 狀況

最近剛入手一台筆電，具備 `Nvidia GeForce RTX 5090 Laptop` 顯卡， VRAM 有 24 GB，想說可以好好跑一下 `Stable Diffusion WebUI Forge` 的軟體了。

就把原本在 RTX 3070 Ti (VRAM 8 GB) 筆電上跑的好好的 `Stable Diffusion WebUI Forge` (以下簡稱 `Forge`) 的軟體，整個資料夾，複製到新筆電上。想說就這樣直接使用看看。

當然，事情沒那麼順利，直接出現一堆 error code。

error code 案例:
```
D:\SdForge\webui_forge_cu121_torch231\webui\venv\lib\site-packages\torch\cuda\__init__.py:209: UserWarning: NVIDIA GeForce RTX 5090 Laptop GPU with CUDA capability sm_120 is not compatible with the current PyTorch installation. The current PyTorch install supports CUDA capabilities sm_50 sm_60 sm_61 sm_70 sm_75 sm_80 sm_86 sm_90. If you want to use the NVIDIA GeForce RTX 5090 Laptop GPU GPU with PyTorch, please check the instructions at https://pytorch.org/get-started/locally/
```

PS: 筆電的作業系統為 Microsoft Windows 11。

# 原因

這個錯誤的核心原因是:

新筆電 GPU 是新的 RTX 5090 Laptop GPU（Blackwell 架構，sm_120），但目前 `Forge` 內建的 PyTorch 版本太舊，不支援 CUDA Compute Capability sm_120。

舊的 `Forge` 使用的 Python 環境:

1. CUDA 12.1
2. Torch 2.3.1

而 Torch 2.3.x 尚未支援 RTX 50 系列。

舊的 `Forge` 是「RTX 40 系列時代」的整合包。RTX 5090 是 2025 新架構，Torch 2.3 根本還沒認識它。

RTX 5090 Laptop 屬於：
- Blackwell 架構
- sm_120

很多舊 AI WebUI：
- Automatic1111
- Forge 舊整合包
- ComfyUI 舊 portable

都會遇到同樣問題。核心不是 GPU 壞掉，而是「PyTorch 太舊」。

# 操作方式概要

測試過幾個解法後，將成功的方式整理如下:

1. 安裝 Python 3.10.x (x >= 9)
2. 升級 NVIDIA Driver (>= 572.XX)
3. 升級 `PyTorch`（至少 2.6+）
4. 移除 `xFormers`
5. 修改 `webui-user.bat` 檔案

# 詳細操作方式

接下來一步一步操作

1. 升級 NVIDIA Driver。

該步驟在 Windows 直接按右下角的 NVIDIA 符號，進去找驅動程式更新就搞定了。

2. 進入 `Forge` 的安裝資料夾後，使用 `cmd`。

```bash
D:\SdForge\webui_forge_cu121_torch231maxnb\webui
```

3. 先確認 `pip` 的位置

先確認 `pip` 的位置。

PS: 這點很重要，因為沒有先確認，讓筆者整整浪費一天在不斷重複安裝，結果只是安裝到錯誤位置。

```bash
where pip
```

結果顯示
```bash
D:\SdForge\webui_forge_cu128_torch211\webui>where pip
C:\Users\hsien\AppData\Local\Programs\Python\Python310\Scripts\pip.exe
```
這表示，路徑連結到主系統的 Python 了。(主系統 Python 安裝在 C 槽)

必須看到 `pip` 是出現在 Python 虛擬環境中的資料夾才對 (類似下面的訊息)。如果不是，代表還沒進 venv。
```bash
D:\SdForge\webui_forge_cu121_torch231\webui\venv\Scripts\pip.exe
```
( Python 的虛擬環境 `venv` 安裝在 D 槽)

由於沒有看到 `pip` 是出現在 Python 虛擬環境中的資料夾，因此接下來的安裝動作，前面都要加上 `python -m`，強制將 Python 套件安裝在該虛擬環境下。

NOTE: 
為何要加上 `python -m`，可參考Po文:  
[[Python] 如何建立並使用 Python 的虛擬環境 venv](https://hsienching.github.io/2026/05/21/Python-venv-usage/)

4. 啟動 Python 的 `venv`：
```bash
venv\Scripts\activate
```

5. 移除舊版 PyTorch
```
python -m pip uninstall torch torchvision torchaudio xformers -y
```

6. 安裝新版 PyTorch（重要）

針對 RTX 50 系列，目前建議：
```bash
python -m pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```

這會安裝：
- CUDA 12.8 版 Torch
- 支援 RTX 5090 / sm_120

7. 移除 `xformers`

原來有裝 `xformers`，但後來發現會造成 `Forge` 啟動錯誤。經過測試，直接移除 `xformers` 就對了。

使用下面指令移除 `xformers`
```bash
python -m pip install uninstall xformers -y
```

完全停用 `xformers`。讓 Torch 2.11 使用原生 SDP attention。這才是 RTX 5090 目前最穩方案。

8. 確認 `xformers` 已移除

執行:
```bash
venv\Scripts\python.exe -m pip list
```

確認 `xformers` 不在列表裡。

9. 驗證 PyTorch 安裝狀況

使用指令:
```bash
venv\Scripts\python.exe -c "import torch; print(torch.__version__)"
```

應該會看到:
```bash
2.x.x+cu128
```

筆者的狀況:
```bash
D:\SdForge\webui_forge_cu121_torch231\webui>venv\Scripts\python.exe -c "import torch; print(torch.__version__)"
2.11.0+cu128
```

10. 驗證 CUDA 支援

使用指令:
```bash
venv\Scripts\python.exe -c "import torch; print(torch.cuda.get_device_name(0))"
```

應該會看到：
```bash
NVIDIA GeForce RTX 5090 Laptop GPU
```

```bash
D:\SdForge\webui_forge_cu121_torch231\webui>venv\Scripts\python.exe -c "import torch; print(torch.cuda.get_device_name(0))"
NVIDIA GeForce RTX 5090 Laptop GPU
```

11. 修改 `webui-user.bat` 檔案

在 `webui-user.bat` 檔案中的 `COMMANDLINE_ARGS=`，改成
```bat
set COMMANDLINE_ARGS=--cuda-malloc --opt-sdp-attention --opt-channelslast
```

12. 重新啟動 `Forge`

正常情況下:
- 不會再出現 sm_120 錯誤
- CUDA 可正常啟用
- 5090 能正常推圖

筆者做到這裡，已經可以正常出圖了。

成功修復 RTX 5090 與 Forge 的相容性問題。

# 為什麼這是可行的解決方案?

## 關於不裝 `xformers`

因為 Torch 2.11 本身已內建超強 attention

在 RTX 5090 上:

| 技術	             | 狀態                          |
| ----------------- | ----------------------------- |
| xformers          | 很多 kernel 尚未支援 Blackwell |
| Torch SDP	        | 官方原生支援                   |
| Flash Attention 2	| 部分支援                       |
| Triton            | 還在成熟中                     |

所以現在， RTX 5090 最穩定方案其實是，不用 `xformers`。

但很多人會誤解，以為有 `xformers` 一定比較快。

但在 Torch 2.6 + RTX 50 系列下，其實 Torch SDP 已經非常接近甚至更穩。

## 關於不裝 `Triton`

Triton 也先不要裝

因為 Blackwell Windows 生態還在變，目前最重要的是穩定。

主要在生圖時:
- 不再出現 `xformers` 錯誤
- SDXL 能正常跑
- RTX 5090 VRAM 正常使用
- 速度其實不會太差

如果之後還出現 attention 相關錯誤

再去 `webui-user.bat` 檔案中的 `COMMANDLINE_ARGS=` 加入
```bat
--disable-attention-upcast
```
或加入
```bat
--precision full --no-half
```
但通常先不用。

# QA: RTX 5090 常見額外問題 (處理過程中的暫時解決方案)

1. `xformers` 爆炸

如果遇到 error code:
```bash
xformers not available
```

可先在 `webui-user.bat` 檔案中的 `COMMANDLINE_ARGS=` 加入 `--disable-xformers`
```bat
set COMMANDLINE_ARGS=--disable-xformers
```

之後再慢慢處理。

2. `Triton` 問題

若啟動 `Forge` 時，出現警告訊息:
```bash
No module named triton
```

則安裝:
```bash
pip install triton-windows
```

3. Flash Attention 問題

50 系列顯卡有時會出現警告訊息:
```bash
FlashAttention not supported
```

可先在 `webui-user.bat` 檔案中的 `COMMANDLINE_ARGS=` 加入 `--opt-sdp-no-mem-attention`
```bat
set COMMANDLINE_ARGS=--opt-sdp-no-mem-attention
```

先關掉，之後再慢慢處理。

# 相關資源
stable-diffusion-webui-forge 的 GitHub:  
https://github.com/lllyasviel/stable-diffusion-webui-forge
