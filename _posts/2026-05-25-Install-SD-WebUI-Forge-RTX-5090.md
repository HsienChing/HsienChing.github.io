---
title: "[AI] 如何讓 Stable Diffusion WebUI Forge 使用 RTX 5090 顯卡 (重新安裝方式)"
date: 2026-05-25 00:10:00 +0800
excerpt: "最近剛入手一台筆電，具備 Nvidia GeForce RTX 5090 Laptop 顯卡， VRAM 有 24 GB，想說可以好好跑一下 Stable Diffusion WebUI Forge 的軟體了。"
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
  - GitHub
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

# 狀況

最近剛入手一台筆電，具備 `Nvidia GeForce RTX 5090 Laptop` 顯卡， VRAM 有 24 GB，想說可以好好跑一下 `Stable Diffusion WebUI Forge` (以下簡稱 `Forge` ) 的軟體了。

之前，已經修復相容性問題，讓原本在 RTX 3070 Ti Laptop 顯卡能跑的 `Forge`，調整到能在 RTX 5090 Laptop 顯卡上跑。
(可參考Po文: [[AI] 如何讓 Stable Diffusion WebUI Forge 使用 RTX 5090 顯卡 (舊檔修理方式)](https://hsienching.github.io/2026/05/24/Repair-SD-WebUI-Forge-RTX-5090/))

這次的方式，將從 GitHub 開始，安裝一套全新的 `Forge`。

PS: 筆電的作業系統為 Microsoft Windows 11。

# 安裝方式概要

1. 一般安裝流程
2. 安裝時的錯誤處理

# 一般安裝流程 - 詳細說明

1. 開新資料夾

    例如:
    ```bash
    D:\AI\ForgeNew
    ```

2. 安裝 git (如果還沒有裝的話)

    可參考 Po 文:
    [[學習] 使用git CLI並與GitHub共同運作(基礎篇)](https://hsienching.github.io/2024/05/16/Use-git-CLI-with-GitHub/)

3. 安裝 Python 3.10

    安裝 [Python 3.10.11](https://www.python.org/downloads/release/python-31011/)

    安裝時，記得勾選 `Add Python to PATH`

4. 從 GitHub clone `Forge` 下來

    在資料夾 `D:\AI` 中，打開 `cmd`，輸入指令，將 GitHub 上的 `Forge` clone 下來
    ```bash
    git clone https://github.com/lllyasviel/stable-diffusion-webui-forge.git ForgeNew
    ```
    這樣在資料夾 `D:\AI\ForgeNew` 中，就有 `Forge` 的檔案了。

5. 執行 `webui-user.bat`

    進入資料夾 `D:\AI\ForgeNew` 中，執行 `webui-user.bat`

    第一次執行時，會有幾個主要動作
    - 自動建立 Python 的虛擬環境 `venv`
    - 安裝 Python 的基礎套件
    - 下載 Torch（但通常版本不夠新）

    等它跑完一次，將 `venv` 建立完成。即使失敗也沒關係。

6. 關閉 `Forge`

    直接關掉 `cmd`

7. 移除舊的 Torch (針對 RTX 5090)

    進入資料夾 `D:\AI\ForgeNew` 中，執行:
    ```bash
    venv\Scripts\python.exe -m pip uninstall torch torchvision torchaudio xformers -y
    ```

8. 安裝新的 Torch (針對 RTX 5090)

    安裝 Torch 2.11 cu128:
    ```bash
    venv\Scripts\python.exe -m pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
    ```

9. 修改 `webui-user.bat` 檔案

    在 `webui-user.bat` 檔案中的 `COMMANDLINE_ARGS=`，改成
    ```bat
    set COMMANDLINE_ARGS=--cuda-malloc --opt-sdp-attention --opt-channelslast
    ```

10. 重新啟動 `Forge`

    期待可以順利啟動，但遭遇了一堆錯誤。

    接下來就是處理錯誤的方式。

# 安裝時的錯誤處理 

錯誤訊息:
```bash
ModuleNotFoundError: No module named 'pkg_resources' [end of output] note: This error originates from a subprocess, and is likely not a problem with pip. ERROR: Failed to build 'https://github.com/openai/CLIP/archive/d50d76daa670286dd6cacf3bcd80b5e4823fc8e1.zip' when getting requirements to build wheel
```

代表 setuptools 壞掉 / 太新 / 缺失。

新版 Python + pip 有時會把 `pkg_resources` 拆掉。但 Forge 某些舊安裝腳本還依賴它。

直接升級整套 packaging 工具，使用指令:
```bash
venv\Scripts\python.exe -m pip install -U pip setuptools wheel
```

升級過程中，又出現錯誤訊息:
```bash
D:\AI\ForgeNew>venv\Scripts\python.exe -m pip install -U pip setuptools wheel
Requirement already satisfied: pip in .\venv\lib\site-packages (26.1.1)
Requirement already satisfied: setuptools in .\venv\lib\site-packages (65.5.0)
Collecting setuptools
  Using cached setuptools-82.0.1-py3-none-any.whl.metadata (6.5 kB)
Collecting wheel
  Using cached wheel-0.47.0-py3-none-any.whl.metadata (2.3 kB)
Collecting packaging>=24.0 (from wheel)
  Using cached packaging-26.2-py3-none-any.whl.metadata (3.5 kB)
Using cached setuptools-82.0.1-py3-none-any.whl (1.0 MB)
Using cached wheel-0.47.0-py3-none-any.whl (32 kB)
Using cached packaging-26.2-py3-none-any.whl (100 kB)
Installing collected packages: setuptools, packaging, wheel
  Attempting uninstall: setuptools
    Found existing installation: setuptools 65.5.0
    Uninstalling setuptools-65.5.0:
      Successfully uninstalled setuptools-65.5.0
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
torch 2.11.0+cu128 requires setuptools<82, but you have setuptools 82.0.1 which is incompatible.
Successfully installed packaging-26.2 setuptools-82.0.1 wheel-0.47.0
```

這表示 Torch 2.11 與 setuptools 82 不相容。

關鍵在於那句 `torch 2.11.0+cu128 requires setuptools<82`，也就是說， `setuptools` 升太高了， Torch 開始警告相容性。

那就把 `setuptools` 降回 81.x 或 80.x，使用指令:
```bash
venv\Scripts\python.exe -m pip install setuptools==81.0.0
```

成功將 `setuptools` 降回 81.x 或 80.x。
```bash
D:\AI\ForgeNew>venv\Scripts\python.exe -m pip install setuptools==81.0.0
Collecting setuptools==81.0.0
  Using cached setuptools-81.0.0-py3-none-any.whl.metadata (6.6 kB)
Using cached setuptools-81.0.0-py3-none-any.whl (1.1 MB)
Installing collected packages: setuptools
  Attempting uninstall: setuptools
    Found existing installation: setuptools 82.0.1
    Uninstalling setuptools-82.0.1:
      Successfully uninstalled setuptools-82.0.1
Successfully installed setuptools-81.0.0
```

重新啟動 `Forge`，又出現錯誤:
```bash
venv "D:\AI\ForgeNew\venv\Scripts\Python.exe"
Python 3.10.11 (tags/v3.10.11:7d4cc5a, Apr  5 2023, 00:38:17) [MSC v.1929 64 bit (AMD64)]
Version: f2.0.1v1.10.1-previous-669-gdfdcbab6
Commit hash: dfdcbab685e57677014f05a3309b48cc87383167
Installing clip
Traceback (most recent call last):
  File "D:\AI\ForgeNew\launch.py", line 54, in <module>
    main()
  File "D:\AI\ForgeNew\launch.py", line 42, in main
    prepare_environment()
  File "D:\AI\ForgeNew\modules\launch_utils.py", line 443, in prepare_environment
    run_pip(f"install {clip_package}", "clip")
  File "D:\AI\ForgeNew\modules\launch_utils.py", line 153, in run_pip
    return run(f'"{python}" -m pip {command} --prefer-binary{index_url_line}', desc=f"Installing {desc}", errdesc=f"Couldn't install {desc}", live=live)
  File "D:\AI\ForgeNew\modules\launch_utils.py", line 125, in run
    raise RuntimeError("\n".join(error_bits))
RuntimeError: Couldn't install clip.
Command: "D:\AI\ForgeNew\venv\Scripts\python.exe" -m pip install https://github.com/openai/CLIP/archive/d50d76daa670286dd6cacf3bcd80b5e4823fc8e1.zip --prefer-binary
Error code: 1
stdout: Collecting https://github.com/openai/CLIP/archive/d50d76daa670286dd6cacf3bcd80b5e4823fc8e1.zip
  Using cached d50d76daa670286dd6cacf3bcd80b5e4823fc8e1.zip (4.3 MB)
  Installing build dependencies: started
  Installing build dependencies: finished with status 'done'
  Getting requirements to build wheel: started
  Getting requirements to build wheel: finished with status 'error'

stderr:   error: subprocess-exited-with-error

  Getting requirements to build wheel did not run successfully.
  exit code: 1

  [17 lines of output]
  Traceback (most recent call last):
    File "D:\AI\ForgeNew\venv\lib\site-packages\pip\_vendor\pyproject_hooks\_in_process\_in_process.py", line 389, in <module>
      main()
    File "D:\AI\ForgeNew\venv\lib\site-packages\pip\_vendor\pyproject_hooks\_in_process\_in_process.py", line 373, in main
      json_out["return_val"] = hook(**hook_input["kwargs"])
    File "D:\AI\ForgeNew\venv\lib\site-packages\pip\_vendor\pyproject_hooks\_in_process\_in_process.py", line 143, in get_requires_for_build_wheel
      return hook(config_settings)
    File "C:\Users\hsien\AppData\Local\Temp\pip-build-env-fg94z4pa\overlay\Lib\site-packages\setuptools\build_meta.py", line 333, in get_requires_for_build_wheel
      return self._get_build_requires(config_settings, requirements=[])
    File "C:\Users\hsien\AppData\Local\Temp\pip-build-env-fg94z4pa\overlay\Lib\site-packages\setuptools\build_meta.py", line 301, in _get_build_requires
      self.run_setup()
    File "C:\Users\hsien\AppData\Local\Temp\pip-build-env-fg94z4pa\overlay\Lib\site-packages\setuptools\build_meta.py", line 520, in run_setup
      super().run_setup(setup_script=setup_script)
    File "C:\Users\hsien\AppData\Local\Temp\pip-build-env-fg94z4pa\overlay\Lib\site-packages\setuptools\build_meta.py", line 317, in run_setup
      exec(code, locals())
    File "<string>", line 3, in <module>
  ModuleNotFoundError: No module named 'pkg_resources'
  [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
ERROR: Failed to build 'https://github.com/openai/CLIP/archive/d50d76daa670286dd6cacf3bcd80b5e4823fc8e1.zip' when getting requirements to build wheel
```

現在可以確定了，問題不是 `setuptools`，而是 `Forge` 這個舊版本的 `CLIP` 安裝流程壞掉了。

尤其 `f2.0.1v1.10.1-previous-669`，這是非常舊的 Forge。它的 `openai/CLIP` 安裝腳本跟現在的 `pip`, `setuptools`, `pyproject`, `build isolation` 已經不相容。

所以現在跑出來的問題是， clone 到的是「Forge 舊世代架構」。也就是: `f2.0.1v1.10.1-previous-669` 版本。這其實是 `Forge` 作者後來保留的相容主線，不是我們想像中的「完全新世代 Forge」。因此這個 `Forge` 的啟動架構還是舊 A1111 系統、requirements 仍偏舊、CLIP 安裝流程也還是舊的。所以才會遇到 `pkg_resources` 這種 2024 遺留問題。

現在只差，手動安裝 CLIP。

在 `D:\AI\ForgeNew` 資料夾中，執行:
```bash
venv\Scripts\python.exe -m pip install --no-build-isolation git+https://github.com/openai/CLIP.git
```

這一步驟，不會進入隔離 build env，不會再缺 `pkg_resources`，能直接成功安裝 CLIP。

然後重新啟動 `webui-user.bat`

到這裡就可以跑圖了。

# 針對 RTX 5090 的 `webui-user.bat` 設置

目前建議保持：
```bat
set COMMANDLINE_ARGS=--cuda-malloc --opt-sdp-attention --opt-channelslast
```
不要：
- xformers
- Triton
- FlashAttention hack

# 新 Forge 狀況描述

現在這個版本的 `Forge` (`f2.0.1v1.10.1-previous-669`)，雖然不是「全新架構 Forge」，但已經是目前主流 `Forge`，只是作者仍維持：
- 舊版版本號
- 舊版啟動架構
- 舊版 A1111 相容層

經過一些處理後，即可正常運作。

開啟 Forge 的訊息 log 顯示:
```bash
venv "D:\AI\ForgeNew\venv\Scripts\Python.exe"
Python 3.10.11 (tags/v3.10.11:7d4cc5a, Apr  5 2023, 00:38:17) [MSC v.1929 64 bit (AMD64)]
Version: f2.0.1v1.10.1-previous-669-gdfdcbab6
Commit hash: dfdcbab685e57677014f05a3309b48cc87383167
Launching Web UI with arguments: --cuda-malloc --opt-sdp-attention --opt-channelslast
Using cudaMallocAsync backend.
Total VRAM 24463 MB, total RAM 64957 MB
pytorch version: 2.11.0+cu128
Set vram state to: NORMAL_VRAM
Device: cuda:0 NVIDIA GeForce RTX 5090 Laptop GPU : cudaMallocAsync
VAE dtype preferences: [torch.bfloat16, torch.float32] -> torch.bfloat16
CUDA Using Stream: False
W0519 16:03:48.590000 23252 venv\Lib\site-packages\torch\distributed\elastic\multiprocessing\redirects.py:29] NOTE: Redirects are currently not supported in Windows or MacOs.
Using pytorch cross attention
Using pytorch attention for VAE
ControlNet preprocessor location: D:\AI\ForgeNew\models\ControlNetPreprocessor
2026-05-19 16:03:52,479 - ControlNet - INFO - ControlNet UI callback registered.
Model selected: {'checkpoint_info': {'filename': 'D:\\AI\\ForgeNew\\models\\Stable-diffusion\\waiIllustriousSDXL_v160.safetensors', 'hash': '4748a7f6'}, 'additional_modules': [], 'unet_storage_dtype': None}
Using online LoRAs in FP16: False
Running on local URL:  http://127.0.0.1:7860

To create a public link, set share=True in launch().
Startup time: 11.6s (prepare environment: 3.0s, launcher: 0.3s, import torch: 4.9s, initialize shared: 0.1s, other imports: 0.1s, load scripts: 1.1s, create ui: 1.4s, gradio launch: 0.7s).
Environment vars changed: {'stream': False, 'inference_memory': 1024.0, 'pin_shared_memory': False}
[GPU Setting] You will use 95.81% GPU memory (23438.00 MB) to load weights, and use 4.19% GPU memory (1024.00 MB) to do matrix computation.
```

這些訊息表示，這套環境現在已經非常健康了。

目前的狀態其實已經是，RTX 5090 + Forge 的高穩定配置。

關鍵訊息都正常：

| 項目             | 狀態 |
| ---------------- | --- |
| Torch 2.11 cu128 | ✅ |
| RTX 5090         | ✅ |
| SDP attention    | ✅ |
| cudaMallocAsync  | ✅ |
| channelslast     | ✅ |
| xformers         | 已安全停用 ✅ |
| VAE bf16         | ✅ |
| Forge 啟動       | ✅ |
| SDXL 載入        | ✅ |

從啟動訊息來看
```bash
Launching Web UI with arguments: --cuda-malloc --opt-sdp-attention --opt-channelslast
```
是目前 Blackwell（RTX 50 系列）非常理想的配置。

`attention pipeline` 部分，目前顯示:
```bash
Using pytorch cross attention
Using pytorch attention for VAE
```
代表 `Forge` 正在使用 Torch 原生的 SDP。這是目前最穩、最相容、最不容易炸 VRAM 的方案。

這個啟動時間其實很漂亮， `Startup time: 11.6s`，對 RTX 5090 + SDXL + Forge 來說算很不錯了。

記憶體 VARM 使用方面， `use 95.81% GPU memory` ， `Forge` 會很積極吃 VRAM。這是正常的。

目前的最佳實務，保持以下列出的狀況
| 元件         | 建議        |
| ------------ | ---------- |
| Torch	       | 2.11 cu128 |
| Attention    | SDP        |
| xformers     | 不裝       |
| Triton       | 不裝       |
| CUDA malloc  | 開         |
| channelslast | 開         |

目前這套 `Forge` 已可穩定使用
- SDXL
- Pony
- Illustrious
- LoRA
- ADetailer
- ControlNet
- 高解析度
- Inpaint

# 額外建議（可選）

1. 之後可以考慮 `webui-user.bat` 的 `COMMANDLINE_ARGS=` 再加 `--pin-shared-memory`。

    但啟動 log 顯示 `pin_shared_memory: False`，表示目前其實沒問題， RTX 5090 + 64GB RAM 已經很夠。

2. 如果之後玩 Flux 或是超大模型，可考慮 `--disable-smart-memory`，有時會更穩。但 SDXL 通常不用。

3. 建議不要隨便更新 `Forge`

    尤其是 `xformers`, `Triton`, `Torch nightly`，因為 RTX 5090 / Blackwell 生態目前仍在快速變動。

4. 如果未來多開程式、開遊戲、開其他 CUDA app，導致 OOM、黑圖、driver reset。

    可以手動限制：
    ```bash
    Settings → Memory management
    ```
    或在 `webui-user.bat` 的 `COMMANDLINE_ARGS=` 再加入 `--reserve-vram 2`，保留 2GB VRAM 給系統。

5. 備份這套 `venv`

    因為現在這套 `Forge` 已經驗證可正常運作。

    建議直接備份整個 Python 的虛擬環境資料夾 `D:\AI\ForgeNew\venv`。

6. 凍結 requirements

    在 `D:\AI\ForgeNew` 資料夾下，執行:
    ```bash
    venv\Scripts\python.exe -m pip freeze > requirements-working.txt
    ```

    凍結 requirements 後，如果哪天更新炸掉，可以直接還原。

# 多個 `Forge` 環境配置

在實際工作中，我們可以配置多個 `Forge`，方便進行不同用途的運算。

例如: 目前已經在 RTX 5090 laptop 顯卡的筆電上配置兩套 `Forge` 系統。

| 環境     | 用途                       |
| -------- | ------------------------- |
| 舊 Forge | 穩定生產                   |
| 新 Forge | 新模型 / 新 extension 測試 |

# 相關 Po 文

[[AI] 如何讓 Stable Diffusion WebUI Forge 使用 RTX 5090 顯卡 (舊檔修理方式)](https://hsienching.github.io/2026/05/24/Repair-SD-WebUI-Forge-RTX-5090/)

# 相關資源
stable-diffusion-webui-forge 的 GitHub:  
https://github.com/lllyasviel/stable-diffusion-webui-forge
