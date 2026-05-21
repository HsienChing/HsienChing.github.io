---
title: "[Python] 如何建立並使用 Python 的虛擬環境 venv"
date: 2026-05-21 16:00:00 +0800
excerpt: "建立 Python 的虛擬環境 venv，並討論常遭遇到的安裝錯誤狀況。"
categories: 
  - Study
  - 學習
tags:
  - Python
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

# 1. 為什麼要用 Python 的虛擬環境 venv ?

Python 的虛擬環境 (virtual environment) 能夠讓每個專案 (project) 或是 應用程式 (Application) 使用獨立的 Python 套件版本，避免以下問題:

1. 避免安裝污染及版本衝突。不同專案可能需要不同版本的套件。例如: A 專案要用 Numpy==1.0.0，B 專案要用 Numpy==2.4.0，如果都裝在同一個地方會打架。
2. 容易部署。requirements.txt 可以用來還原乾淨的環境。

# 2. 使用方式 - 懶人包

1. 建立虛擬環境

進入到想要使用虛擬環境的專案資料夾中，建立虛擬環境

```bash
python -m venv venv
```

2. 啟用虛擬環境

在 macOS/Linux 系統中:
```bash
source <venv>/bin/activate
```

在 Windows 系統中，使用 cmd:
```bash
venv\Scripts\activate
```

3. 安裝套件

```bash
python pip install <application>
```

其中，`<application>` 為套件名稱。

4. 關閉虛擬環境

```bash
deactivate
```

# 3. Python 官方文件對虛擬環境的描述

`venv` 模組支援建立輕量級的「虛擬環境」，每個虛擬環境都有獨立安裝在其 site 目錄中的一組 Python 套件。虛擬環境是建立在既有的 Python 安裝上（稱為該虛擬環境的「基底」Python）之上，預設會與基底環境中的套件隔離，使得僅能存取明確安裝在虛擬環境中的套件。

在使用虛擬環境中時，像 `pip` 這樣的常用安裝工具會自動將 Python 套件安裝至虛擬環境中，而不需要額外指定。(筆者: 但有時會出狀況，直接使用 `pip` 指令時，卻將套件安裝在主系統中，造成主系統的 Python 被汙染。使用前請確認狀況。)

虛擬環境的主要的特性:
- 用來包含特定的 Python 直譯器、函式庫與二進位檔案，以支援某個專案 (函式庫或應用程式)。預設情況下，這些內容會與其他虛擬環境中的軟體以及作業系統中安裝的 Python 直譯器與函式庫隔離。
- 被包含在一個目錄中，通常命名為專案目錄下的 `.venv` 或 `venv`，也可能是在用來管理多個虛擬環境的容器目錄中，例如 `~/.virtualenvs`。
- 不應提交至 `git` 等版本控制系統。
- 被視為可拋棄的資源——應能簡單地刪除並從頭重新建立。請勿將任何專案程式碼放置於虛擬環境中。
- 不應移動或複製——若需在其他位置使用，應重新建立相同的虛擬環境。

更多關於 Python 虛擬環境的背景資訊請見
1. [venv --- 建立虛擬環境](https://docs.python.org/zh-tw/3/library/venv.html)
2. [PEP 405 – Python Virtual Environments](https://peps.python.org/pep-0405/)
3. [Install packages in a virtual environment using pip and venv](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#create-and-use-virtual-environments)

# 4. 如何建立 Python 的虛擬環境 venv

Python 的虛擬環境是透過執行 `venv` 模組來建立:

```bash
python -m venv /path/to/new/virtual/environment
```

執行此命令會建立目標目錄 (同時也會建立任何還不存在的父目錄) 並在目錄中放置一個名為 `pyvenv.cfg` 的檔案，其中包含一個指向執行該命令的 Python 安裝路徑的 home 鍵。它同時會建立一個名稱為 `bin` 的子目錄 (在 Windows 上為 `Scripts`)，其中包含一個 Python 二進位檔案的副本/符號連結 (根據建立環境時使用的平台或引數而定)。此外，它還會建立一個 `lib/pythonX.Y/site-packages` 子目錄 (在 Windows 上為 `Lib\site-packages`)。如果指定的目錄已存在，則將重新使用該目錄。

# 5. 如何啟用 Python 的虛擬環境 venv

Python 的虛擬環境可以透過位於二進位檔案目錄中的腳本「啟用」(在 Linux/macOS 上為 `bin`；在 Windows 上為 `Scripts`) 這會將該目錄加入到你的 `PATH`，當你運行 python 時就會叫用該環境的直譯器並且執行已安裝的腳本，而不需要使用完整的路徑。

啟動腳本的方式因作業系統平台而異（其中指令中的 `<venv>` 需要替換成包含虛擬環境的目錄路徑）

在 macOS/Linux 系統中:
```bash
source <venv>/bin/activate
```

在 Windows 系統中，使用 `cmd`:
```bash
venv\Scripts\activate
```

在 Windows 系統中，使用 `PowerShell`:
```bash
venv\Scripts\Activate.ps1
```

PS:  
其他系統的 Python venv 啟動指令，可在官方文件中查到  
[venv --- 建立虛擬環境](https://docs.python.org/zh-tw/3/library/venv.html)

啟動成功後，命令列會出現類似 `(venv)` 的提示符。說 "類似" 是因為這個虛擬環境的名稱是由我們自己定義。

若我們的啟用指令為

```bash
python -m venv ml
```

則啟動成功後，命令列會出現類似 `(ml)` 的提示符。

# 6. 安裝 Python 套件

使用 `pip` 進行 Python 套件安裝。
這裡的指令較為嚴謹，在 `pip` 前面加上 `python -m` 確保將套件安裝在當前使用的 Python 環境中，避免多版本衝突。
```bash
python -m pip install <application>
```

PS: 可使用 `where pip` 指令，來確定套件安裝資料夾。若正常狀況下，輸出結果會看到專案的路徑。若不正常，則會看到其他路徑，例如: 主系統 Python 的安裝路徑。

若不確定 `pip` 的路徑，為了避免將套件安裝到其他地方，例如: 安裝到主系統的 Python (筆者遭遇過這種窘況，安裝N次後，浪費一天時間才發現)。建議 `pip` 前面要加入 `python -m`，強制安裝在目前使用的 Python 環境中。

# 7. 依賴列表

在 Python 虛擬環境中，pip 不要求我們逐一安裝套件，而是允許在 requirements.txt 檔案中聲明所有依賴套件。例如，我們可以建立一個名為 `requirements.txt` 的文件 (或使用其他自訂名字)，該文件的內容如下:

```
requests==2.18.4
google-auth==1.1.0
```

使用以下指令建立依賴列表
```bash
pip freeze > requirements.txt
```

然後使用 `-r` 標誌告訴 pip 安裝此檔案中列出的所有套件:
```bash
pip install -r requirements.txt
```
這樣有個好處，只需將 `requirements.txt` 加入版本控制，其他人也能重建環境。

PS: 該檔案習慣通常放在 `venv` 資料夾的上一層。

# 8. 關閉虛擬環境

在虛擬環境中，執行完所需要的動作後，可關閉並離開虛擬環境。

```bash
deactivate
```

# 9. 刪除虛擬環境

直接刪除虛擬環境的資料夾即可。

或者，在 `deactivate` 虛擬環境後，直接將直接刪除虛擬環境的資料夾即可。

# 10. 不要將虛擬環境資料夾加入版本管理軟體 (例如: git)

不要將虛擬環境資料夾 (例如: `venv/`)加入版本管理軟體 (例如: `git`)

以 `git` 為例，在 `.gitignore` 檔案中，加入下列內容。

```
# Python virtual environment
venv/
.env/
*.pyc
__pycache__/
```

# 11. 參考資料

1. [Python Virtual Environments: A Primer](https://realpython.com/python-virtual-environments-a-primer/)  
有詳盡的 Python 虛擬環境使用介紹
2. [venv --- 建立虛擬環境](https://docs.python.org/zh-tw/3/library/venv.html)
3. [PEP 405 – Python Virtual Environments](https://peps.python.org/pep-0405/)
4. [Install packages in a virtual environment using pip and venv](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#create-and-use-virtual-environments)
