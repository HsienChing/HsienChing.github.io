---
title: "[Git][GitHub][使用情境討論] 建了一個 GitHub 的 Repository，並與多台電腦上使用這個 Repository"
date: 2026-05-22 11:00:00 +0800
excerpt: "[使用情境討論] 建了一個 GitHub 的 Repository，並與多台電腦上使用這個 Repository。"
categories: 
  - Study
  - 學習
tags:
  - Git
  - GitHub
  - Multi-computer
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

# 1. 簡介

建了一個 GitHub 的 Repository，並與多台電腦上使用這個 Repository。

以下討論一些， `git` 的使用情境，並附上指令碼，做為參考。

# 2. 情境1: 從 GitHub 上，將 Repository 下載回來使用

首先從 GitHub 的 Repository 上，`clone` 檔案到本機使用

```bash
git clone <Repository.git>
```

# 3. 情境2: 專案中的檔案修改好後，此時本機的版本是最新的，要上傳 GitHub，更新 GitHub 上的檔案

專案中的檔案修改好後，此時本機的版本是最新的，要上傳 GitHub，更新 GitHub 上的檔案。

**操作方式:**

確認本機中的檔案狀態
```bash
git status
```

會看到本機中的檔案，有哪些檔案變動的訊息。

把所有變動的檔案加入追蹤，將內容加到暫存區
```bash
git add .
```

或者將 `git status` 所列出有變動的檔案加入追蹤，並將內容加到暫存區。也就是，挑自己想要的幾個檔案放到 `git add` 指令後面。(直接複製 `git status` 列出的 `路徑/檔名`，再貼到 `git add` 指令後面)

將暫存區的內容提交到儲存庫（Repository）保留
```bash
git commit -m "Update data"
```

將變動的檔案 push 到 GitHub (上傳到 GitHub)
```bash
git push
```

# 4. 情境3: 當 GitHub 上的版本是最新版時，更新本機中的檔案到最新版

在 A 及 B 兩台電腦都下載 Repository 後，先在 A 電腦修改檔案後， push 上 GitHub (上傳到 GitHub)。這時候， GitHub 上及電腦 A 的檔案都是最新版本。而電腦 B 的版本比較舊。

接著在 B 電腦 pull 下 GitHub 的最新版檔案，將 B 電腦中的檔案更新到 GitHub 上的最新版本，繼續進行專案作業。

**操作方式:**

1. 同步並合併 (最常用)。但自己得非常確認 GitHub 上的檔案是最新版本。

    直接抓取 GitHub 上的最新版本並合併到 B 電腦中目前的本地分支:

    ```bash
    git pull origin <分支名稱>
    ```
    (例如：git pull origin main 或 git pull origin master)

2. 僅抓取不合併 (最安全)。

    若想先確認遠端進度再決定是否合併

    先抓取 GitHub 上的最新版本，但此時尚未合併檔案。
    ```bash
    git fetch origin
    ```

    使用以下指令比對遠端與本地的差異
    ```bash
    git diff origin/<分支名稱>
    ```

    看完差異後，按 `q` 退出。

    若確認無誤，再將其合併
    ```bash
    git merge origin/<分支名稱>
    ```

    合併後，就可以繼續專案處理。當有檔案要存檔，並更新 GitHub 上時，就回到 "情境2" 的操作。

PS: 上述指令，其實沒打 <分支名稱> 也可以執行。

# 5. 相關Po文
1. [[學習] 使用git CLI並與GitHub共同運作(基礎篇)](https://hsienching.github.io/2024/05/16/Use-git-CLI-with-GitHub/)
2. [[學習] 使用.gitignore檔案讓git忽略特定的檔案或資料夾，不要進行版本管理。](https://hsienching.github.io/2024/05/18/Use-.gitignore-File-to-Let-git-Ignore-Files/)
