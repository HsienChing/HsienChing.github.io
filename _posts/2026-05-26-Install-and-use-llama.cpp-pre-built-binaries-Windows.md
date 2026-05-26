---
title: "[AI] 在 Windows 系統上，安裝並使用 llama.cpp 的 pre-built binaries 版本"
date: 2026-05-26 00:10:00 +0800
excerpt: "最近剛入手一台筆電，具備 Nvidia GeForce RTX 5090 Laptop 顯卡， VRAM 有 24 GB，想說可以好好跑一下大型語言模型了，於是就來安裝 llama.cpp 的 pre-built binaries 版本。"
categories: 
  - Study
  - 學習
tags:
  - AI
  - Windows
  - llama.cpp
  - HuggingFace
  - RTX5090
  - Nvidia
  - GitHub
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

# 狀況

最近剛入手一台筆電，具備 `Nvidia GeForce RTX 5090 Laptop` 顯卡， VRAM 有 24 GB，想說可以好好跑一下大型語言模型了，於是就來安裝 `llama.cpp`。

PS: 筆電的作業系統為 Microsoft Windows 11。

# 安裝流程

1. 安裝 CUDA

    由於該筆電的顯卡是 RTX 5090 ，於是安裝 [CUDA 13.1](https://developer.nvidia.com/cuda-13-1-0-download-archive) 

2. 下載 `llama.cpp`

    由於該筆電是 Windows 11 的系統，且顯卡是 RTX 5090 ，於是下載 `llama.cpp` 的 `Windows x64 (CUDA 13)` 版本

    下載位置:  
    進去 `llama.cpp` 的 GitHub 官網後，點擊 release ，可進入頁面，看到各作業系統/各種CPU/GPU的 `llama.cpp` pre-built binaries 版本。  
    [GitHub: ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)

    下載完後，進行解壓縮。

    進入 `llama.cpp` 解壓縮後的資料夾後，資料夾中的 `llama-server.exe` 就是 `llama.cpp` 的執行檔。

    PS: 此時若點擊 `llama-server.exe` 則會啟動 `llama.cpp`。但此時沒有安裝模型，其實只有看到 `llama.cpp` 提供的網頁聊天介面，但還無法使用。
    
3. 下載模型，並置於 `llama.cpp` 中

    下載模型，並置於 `llama.cpp` 下層的資料夾中。建議在 `llama.cpp` 下層建立一個 `model` 資料夾，將下載好的模型放入這個資料夾中。(PS: 這是一種習慣性的建議，使用者當然可以自訂自己想要的資料夾名稱及路徑。)

    目前，很多模型都放在 [`Hugging Face`](https://huggingface.co) 平台上，可由該平台下載合適的模型。

# 如何使用 `llama.cpp` 運行大型語言模型

## 如何啟動 GGUF 模型

進入 `llama.cpp` 資料夾後，使用 `cmd` 執行指令:
```bash
llama-server.exe -m "models\<想要執行模型.gguf>" -ngl 999
```
其中， `-ngl 999` 代表儘量把模型全部加加載到 GPU 的 VRAM 中。而 `models` 資料夾是我們在 `llama.cpp` 資料夾下自訂的資料夾，可使用自定義名字，只要記得路徑跟檔名要打對即可。

完成啟動後，在瀏覽器網址列輸入 `http://127.0.0.1:8080` ，即可進入 `llama.cpp` 提供的網頁聊天介面，
開始使用模型。

## 如何啟動 GGUF 多模態視覺模型

加載多模態視覺模型需要2個檔案，一個是主要模型檔案，另一個是 `mmproj` 視覺模型檔案。

進入 `llama.cpp` 資料夾後，執行指令:
```bash
llama-server.exe -m "models\<主要模型.gguf>" --mmproj "models\<mmproj視覺模型.gguf>" -ngl 999
```

# 自製 `Start_llamaCPP.bat` 檔，方便執行 `llama.cpp`

每次要啟動 GGUF 模型都要打一次指令，很麻煩。而且要換成不同的 GGUF 模型時，指令也要更換不同的 GGUF 檔名，也很麻煩。

我們可以自製一個 `*.bat` 檔，來達成 "輸入數字就可以啟動目標模型" 的簡易方式。

`*.bat` 檔案格式:
```bat
@echo off
chcp 65001 >nul
cd /d ./<llama.cpp安裝資料夾> 

echo Please select the model:
echo 1. Model A 
echo 2. Model B
echo 3. Model C

set /p choice=Please enter the number:

if "%choice%"=="1" llama-server.exe -m "models\<Model A.gguf>" -ngl 999
if "%choice%"=="2" llama-server.exe -m "models\<Model B.gguf>" --mmproj "models\<mmproj-BF16.gguf>" -ngl 999
if "%choice%"=="3" llama-server.exe -m "models\<Model C.gguf>" -ngl 999

pause
```
其中，
1. `./<llama.cpp安裝資料夾>` 表示 `llama.cpp` 的安裝資料夾路徑。
2. `Model A` 替換成模型名稱或自訂說明。
3. `if "%choice%"=="1" ` 後面要填入執行 `Model A` 的指令。

請將上方的指令根據自己的模型安裝狀況進行修改，並存檔到 `Start_llamaCPP.txt` (檔名可自訂)，建議將該檔案置於 `llama.cpp` 的資料夾上一層。

接著將 `Start_llamaCPP.txt` 另存新檔。存檔時，格式選擇為 `utf-8` (這樣檔案中就可以填寫中文字)，再將副檔名由 `txt` 改為 `bat` 即可。

這樣 `Start_llamaCPP.bat` 就製作完成。點擊兩下，可看到數字選項及文字說明，輸入模型對應的數字，即可啟動對應的模型。

# 什麼是 `llama.cpp` ?

`llama.cpp` 在官方 GitHub 的描述很簡單:
> LLM inference in C/C++

對於 AI 開發者而言， `llama.cpp` 是目前最流行的本地 GGUF 模型推理框架之一。

很多大家熟悉的本地模型，其实都可以透過 `llama.cpp` 執行，例如:
- Gemma
- Hermes
- Qwen
- Llama
- DeepSeek
- Dolphin
- Mistral
- Mixtral

特別是目前 GGUF 生態逐漸成熟，很多模型在發布時，也同時會發布 GGUF 量化版。

而 `llama.cpp` 一個很大的優勢是:
- 輕量
- 支援多平台
- 支援 GPU
- 支援 CPU
- 支援 GGUF

現在甚至已經支援:

- 多模態
- 圖片理解 (導入 vision 模型)
- OpenAI-like API
- 網頁聊天介面

# `llama.cpp` 的進化簡史

許多人第一次嘗試本地的大型語言模型時，最頭大的問題，不是模型本身，而是安裝軟體時的各種環境問題，例如:
- CUDA 版本不匹配
- DLL 缺失
- 驅動程式不相容
- 環境變量錯誤
- `HIP` / `Vulkan` 配置複雜
- CMake 編譯過程報錯
- Windows 編譯過程報錯

對於新手而言，教學都還看得一個頭兩個大，在安裝過程中，就被一堆 eroor code 勸退了。

大約是從 2024 年中後期開始， `llama.cpp` 的 GitHub release 才逐漸穩定提供「多種 Windows 預編譯版本（prebuilt binaries）」給一般使用者直接下載。

如果要問:「哪一版開始比較明顯進入『下載 → 解壓縮 → 直接跑』時代？」

那比較公認的版本分界點其實是:
- 約在版本 b26xx ~ b27xx (2024/04左右) 開始重新恢復與整理 Windows binaries
- 直到版本 b3xxx ~ b4xxx (2024 年中) 已逐漸成熟

真正對「一般使用者友善化」則是:
- 版本 b7xxx 之後
- 特別是 2025 年的 b8xxx～b9xxx 版本

因為這時候官方的 GitHub release 已經固定提供以下幾個 "CPU / GPU" 的支援
- Windows x64 CPU
- CUDA 12 (NVIDIA)
- CUDA 13 (NVIDIA)
- Vulkan
- HIP（AMD）
- SYCL（Intel）

現在的情況確實已經接近「可直接使用」。也就是:

> 下載 zip → 解壓縮 → 雙擊 `llama-server.exe` 直接跑

如果回頭看早期 `llama.cpp` (2023 ~ 2024 初期)，其實幾乎都是:

自己裝:
- Visual Studio
- CMake
- CUDA Toolkit

接著手動 `cmake` 編譯。還要處理:
- OpenBLAS
- cuBLAS
- Vulkan SDK
- PATH
- DLL 缺失

所以以前很多人其實是改用
- LM Studio
- KoboldCpp
- Ollama
來避開編譯地獄。

而現在官方 release 的檔案命名也很清楚，例如:
- llama-b9050-bin-win-cuda-12-x64.zip
- llama-b9050-bin-win-cuda-13-x64.zip
- llama-b9050-bin-win-vulkan-x64.zip
- llama-b9050-bin-win-avx2-x64.zip

意思分別是:
- CUDA 12 GPU版
- CUDA 13 GPU版
- Vulkan GPU版
- 純 CPU AVX2 版

只要挑對版本即可。在使用上相對輕鬆很多。

# 目前 `llama.cpp` 的 Windows 版本支援程度如何?

目前 `llama.cpp` 官方 GitHub 的 Release 已經直接提供多項支援:  
(對其他作業系統的支援，請直接上 [GitHub: ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) 進行查閱。)

- Windows x64 (CPU)
- Windows arm64 (CPU)
- Windows x64 (CUDA 12) - CUDA 12.4 DLLs
- Windows x64 (CUDA 13) - CUDA 13.1 DLLs
- Windows x64 (Vulkan)
- Windows x64 (SYCL)
- Windows x64 (HIP)

這豐富的支援項目意味著

## 對於 NVIDIA 使用者

可以直接選擇 `CUDA 12.4` 或者 `CUDA 13.1` 版本的 `llama.cpp`。

如果使用者的顯卡是 RTX 30、40、50 系列，基本建議選擇 CUDA 版本。

## 對於 Intel 使用者

這讓 Intel 核顯、 Arc 獨顯，都能跑大型語言模型了。

可以試試 `SYCL` 或者 `Vulkan` 版本的 `llama.cpp`。

儘管性能與 NVIDIA 相比，仍有一定差距，但已經能正常跑很多 GGUF 小模型了。

## 對於 AMD 使用者

終於不用完全仰賴 `ROCm`。

可以使用 `HIP` 或者 `Vulkan` 版本的 `llama.cpp`。

很多情况下，`Vulkan` 會比 `HIP` 更穩定。

# 相關 Po 文

[[AI] 如何讓 Stable Diffusion WebUI Forge 使用 RTX 5090 顯卡 (重新安裝方式)](https://hsienching.github.io/2026/05/25/Install-SD-WebUI-Forge-RTX-5090/)

[[AI] 如何讓 Stable Diffusion WebUI Forge 使用 RTX 5090 顯卡 (舊檔修理方式)](https://hsienching.github.io/2026/05/24/Repair-SD-WebUI-Forge-RTX-5090/)

# 相關資源
[GitHub: ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp)
