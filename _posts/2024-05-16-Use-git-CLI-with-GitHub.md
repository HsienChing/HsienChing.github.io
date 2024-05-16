---
title: "[學習] 使用git CLI (基礎篇) 並與GitHub共同運作"
date: 2024-05-16 10:15:00 +0800
excerpt: "簡單講一下，基礎的git CLI操作。"
categories:
  - Study
  - 學習
tags:
  - Solving issues
  - 處理問題
  - Study
  - 學習
  - git
  - GitHub
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

> 沒事不要亂學東西!
>
> 有學習目標時，再針對需求進行學習!
>
> 鍾獻慶

2024-04-17 星期三

這幾天連續手動操作GitHub的repository，對同一個檔案，改來改去，一直搞手動存檔。如果只存一個檔就算了，問題是，當要連續存十幾個檔，或上百個檔時，每個檔案都手動存檔，這樣搞起來，很累。

發現這樣很沒有效率。

看起來還是要學一下`git`這個版本管理程式，並使用`git`來對GitHub的repository進行操作，這樣效率會比較好。

(此文不採用線性編排，是給實務使用者進行指令參考。若要學習`git`的基本ABC，請參考其他資料。)

# 前置作業

要在自己的電腦上安裝`git`這個版本管理軟體。

直接去Git官網下載並安裝就行。

Git官網: <https://git-scm.com/>

# 概念

使用`git`作為版本管理軟體，來管理專案。

什麼是`專案`?

在程式設計中的專案，是用一個資料夾開始，裡面放相關檔案。  
例如: 建立一個`GitTest`在D槽下。相關檔案就放在這個資料夾下。

而使用`git`，就是方便管理這個專案資料夾下面的檔案。

NOTE: 在GitHub上，這個專案資料夾稱為repository。

# 開始使用git進行本機上的專案管理

## Command: git init

進入`GitTest`資料夾後，使用command `git init`將這個資料夾初始化為`Git repository`。

Command:
```bash
$ git init
```

Result:
```bash
Initialized empty Git repository in D:/GitTest/.git/
```

這個command會在專案目錄(Git repository)下，建立一個`.git`的隱藏資料夾。裡面存放`git`文件。

NOTE: 如果要取消`git`專案管理，直接刪除這個隱藏資料夾就行。

## Command: git status

使用command `git status`確認狀況。

```bash
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

由於目前專案資料夾中沒有內容，所以就顯示這樣。

## 在專案資料夾下建立檔案 (開始進行專案的意思)

開始進行專案後，我們就會在專案資料夾下建立檔案。

例如: 建立一個`index.html`檔，裡面先不打字。

使用command `git status`確認狀況。

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index.html

nothing added to commit but untracked files present (use "git add" to track)
```

顯示`index.html`這個檔案在`Untracked files`的列表中。

## Command: git add [file]

使用command `git add [file]`可以對檔案進行追蹤(track)

```bash
$ git add index.html
```

按下`Enter`後，並不會顯示資訊。

需使用command `git status`來確認狀況。

```bash
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   index.html
```

這時`Changes to be committed`的檔案就新增了一個`index.html`

NOTE: 若嫌一個檔案一個檔案慢慢加很慢，可以用`git add --add`或`git add .`，一次全部加入。

## Command: git rm --cached <file>

若要取消這個檔案列於`Changes to be committed`

就使用command `git rm --cached <file>`

```bash
$ git rm --cached index.html
rm 'index.html'
```

使用command `git status`來確認狀況。

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index.html

nothing added to commit but untracked files present (use "git add" to track)
```

使用command `git add`對檔案進行追蹤(track)。

```bash
$ git add index.html
```

使用command `git status`確認狀況。

```bash
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   index.html
```

## Command: git log

使用command `git log`確認有沒有`log`檔。

```bash
$ git log
fatal: your current branch 'master' does not have any commits yet
```

雖然有做`git init`，但還沒`commit`，所以還沒有`log`檔。


使用command `git commit -m "[description text]`來進行commit。

```bash
$ git commit -m "git init"
[master (root-commit) ec13f26] git init
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 index.html
```


-----

## 使用command `git config --global --list`確認`user.email`跟`user.name`

使用command `git config --global --list`可以看到`user.email`跟`user.name`的值。
若要進行comment動作，這兩個需要有值，git需要知道是誰要進行commit的動作。

```bash
$ git config --global --list
user.email=[value of user e-mail]
user.name=[value of user name]
```

使用command `git config --global user.email "[value of user e-mail]"`可以修改`user.email`的值為[value of user name]。  
使用command `git config --global user.name "[value of user name]"`可以修改`user.name`的值為[value of user name]。

```bash
$ git config --global user.email "GOOD@gmail.com"

$ git config --global user.name "GOOD"

$ git config --global --list
user.email=GOOD@gmail.com
user.name=GOOD
```

來做個小測試:

將`user.email`跟`user.name`的值都設定為空白`""`。

```bash
$ git config --global user.name ""

$ git config --global user.email ""

$ git config --global --list
user.email=
user.name=
credential.helper=store
```

此時將無法進行commit，git會問"你是誰?"，並提供修改`user.email`跟`user.name`的方式。

```bash
$ git commit -m "commit test"
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <>) not allowed
```

-----



另外，使用command `git log`確認有沒有`log`檔。如果有的話，就會看log檔內容。

```bash
$ git log
commit ec13f26c40ab4c962dc180d7bbf704c84cd41d41 (HEAD -> master)
Author: [value of user name] <[value of user e-mail]>
Date:   Wed Apr 17 21:57:09 2024 +0800

    git init
```

去看`.git/refs/heads/master`這個檔案，也會看到master的值。

```bash
$ cat .git/refs/heads/master
073eaa43a0a0251b4cf6a25f8e66464268401209

$ git log
commit 073eaa43a0a0251b4cf6a25f8e66464268401209 (HEAD -> master)
```

-----

# 與GitHub一起使用

## 在GitHub上建立repository

在GitHub上建立repository，名稱，例如: `RubyOnRailsTest`

在`.gitignore`欄位選擇合適的template，這個選項是方便忽略不要進行版本管理的檔案。當然，也可以自己手動寫入要忽略不進行版本管理的檔案。

## 使用git去複製(clone) GitHub上面的repository

打開本機的CMD `git bash`，進去目標專案資料夾`GitTest02`。

使用command `git clone [GitHub提供的link]`將GitHub上`RubyOnRailsTest`的repository裡面的檔案全部下載到`GitTest02`資料夾下面的`RubyOnRailsTest`資料夾裡面。

```bash
$ git clone [GitHub提供的link]
Cloning into 'RubyOnRailsTest'...
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 5 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (5/5), done.
```

NOTE: 使用clone的好處是，它會自動幫本機跟GitHub的repository設定連線。

NOTE: 當完成`git clone`的動作，在本機的`GitTest02`資料夾下面的`RubyOnRailsTest`資料夾名稱可以改變。不會影響後續的`git push`動作。

## 使用command `git push`，將本機上面的檔案上傳到GitHub的repository裡面

當本機的專案資料夾，新增一些檔案或修改檔案後，並commit完後。

使用command `git push`，將本機上面的檔案上傳到GitHub的repository裡面。

```bash
$ git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 20 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 288 bytes | 288.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To [GitHub的repository]
   655304e..23825a3  main -> main
```

NOTE: 這個動作會要求，帳號及密碼。如果已經設定過，就不會顯示這個要求。

---

NOTE:  
剛上傳完後，本機跟GitHub上面的資料內容就都一樣了。

這時候，如果再進行`git push`的動作，則會顯示`Everything up-to-date`，表示兩邊檔案內容都相同，不用進行動作的意思。

```bash
$ git push
Everything up-to-date
```
---

## 使用command `git pull`，將GitHub的repository上面的新增檔案或修改下載到本機的repository裡面

當GitHub的專案資料夾，新增一些檔案或修改檔案後，並commit完後。

使用command `git pull`，將GitHub的repository上面的新增檔案或修改下載到本機的repository裡面。

```bash
$ git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 922 bytes | 46.00 KiB/s, done.
From [GitHub的repository]
   23825a3..8fa7a3f  main       -> origin/main
Updating 23825a3..8fa7a3f
Fast-forward
 Test.rtf | 4 +++-
 1 file changed, 3 insertions(+), 1 deletion(-)
```

# 關於git

> `git` is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
>
> git官網: <https://git-scm.com/>

`git` logo:  
![](https://upload.wikimedia.org/wikipedia/commons/e/e0/Git-logo.svg){:height="30%" width="30%"}

# git雜項資訊

以下，是整理中的資料，有許多是未完全，但仍然可供提示一些查詢跟學習方向。就留在這裡，供參考。

## 分散式的版本控制系統 (distributed version control system)

---

何謂`版本`? 

當我們對repository裡面的檔案進行新增、修改、刪除等動作時，每次的一個(組)動作，就造成一個repository的新`版本`。

---

`git`跟其他的版本控制系統有什麼不同?

`git`只在乎檔案內容，不在乎資料夾或檔案名稱。是個內容追蹤軟體。

---

既然`git`只在乎檔案內容，不在乎資料夾或檔案名稱。那檔名存在哪裡?

存在`tree`的物件中。

---

有些版本控制軟體是紀錄每次的檔案。每多一個版本，就是所有檔案都copy。

而git是拍快照(snapshot)，把資料夾結構拍下來。只要檔案沒有變更，就不會存檔，而是記錄前一次檔案的連結。

---


## `git add --all` vs. `git add .`

在`git` 1.x版的時候

| 使用參數 | 新增檔案 | 修改檔案 | 刪除檔案 |
| :-----: | :-----: | :-----: | :-----: |
| `--all` |    O    |    O    |    O    |
|   `.`   |    O    |    O    |    X    |

在`git` 2.x版的時候

| 使用參數 | 新增檔案 | 修改檔案 | 刪除檔案 |
| :-----: | :-----: | :-----: | :-----: |
| `--all` |    O    |    O    |    O    |
|   `.`   |    O    |    O    |    O    |

`git add --all`，不管在專案的哪一層資料夾執行，都有效果。

`git add .`，只會對目前指令下達的這一層資料夾及其子資料夾，有效果。

REF:  
YouTube: 你知道 Git 是怎麼一回事嗎  
<https://youtu.be/LgTf7m5B0xA?t=257>


## 什麼是`SHA1`演算法?

`git`裡面那個亂亂的碼，是使用`SHA1`演算法計算得來。
```bash
$ git log
commit 073eaa43a0a0251b4cf6a25f8e66464268401209 (HEAD -> master)
```

通常會用40個16進位字元組成。

具備1對1函數特性。  
也就是 1個 input value 對應 1個 output value.

由於兩個不同的檔案，透過`SHA1`演算法計算，會有不同的值。
因此，`SHA1`演算法可以拿來應用。達到類似指紋的目的。

有沒有可能，兩個不同內容的檔案，有同樣的`SHA1`值。這個機率非常低。這個狀況稱為`碰撞`。

Example:

Google暴力修改PDF檔9兆次，讓修改後的PDF檔與原始檔具有相同的`SHA1`值。

REF:  
1. Google Security Blog: [Announcing the first SHA1 collision](<https://security.googleblog.com/2017/02/announcing-first-sha1-collision.html>)
2. [史上第一例！Google破解SHA1實現碰撞攻擊](<https://www.ithome.com.tw/news/112347>)


但`git`計算`SHA1`值時，還有加料。



在`git`中，每個物件都有他的`SHA1`值。


---

## commit時，發生什麼事?


`git`中有4種物件

1. `blob`。跟檔案有關。
2. `tree`。跟資料夾及檔名有關。
3. `commit`。存放commit資訊。
4. `tag`。存放tag。

REF:  
<https://youtu.be/LgTf7m5B0xA?t=909>

---

在repository中，新增一個檔案時。此時該當案處於`untracked`的狀態。

當使用command `git add`把該檔案加入後，就會產生一個`blob`物件。


當使用command `git commit`，`git`就會產生`tree`、`commit`物件。


可以用command `git count-objects`來算物件數量。

---

## 關於reset

reset有3種參數
1. `--mixed`。當沒有下任何參數時，預設使用`--mixed`參數。
2. `--soft`。
3. `--hard`。

REF:  
<https://youtu.be/LgTf7m5B0xA?t=1239>

---

## 關於 `branch` (分支)

有人可能會認為`branch`就是把檔案全部copy出來，改完後，再合併回去。但這樣成本就很高。

> 在`git`裡面開`branch`很便宜。

`branch`只是一個指向某個`commit`的指標。

所謂的`branch`，就是一個有40個字元的檔案。(`SHA1`值) (不用copy一堆檔案，所以很便宜)

在`.git/refs/heads/master`的檔案中，會記錄 master branch。

使用command `git branch`，可以看到目前的repository中有哪些`branch`。

使用command `git branch [branch name]`，可以在目前的repository中新增`branch`。

`branch`的檔案會存在`.git/refs/heads/`資料夾下。每個檔案，表示一個`branch`。  
如果刪除某個檔案，就是刪除某個`branch`。  
如果更改某個檔名，就是更改`branch`的名字。

REF:
<https://youtu.be/LgTf7m5B0xA?t=1269>

---

## 關於 `HEAD`

`HEAD`就是指向某個`branch`的指標。存放在檔案`.git/HEAD`中。

使用command `git checkout [branch name]`可以改變`.git/HEAD`值。

請勿手動改`.git/HEAD`值，不會報錯，但`git`會出錯。

REF:  
<https://youtu.be/LgTf7m5B0xA?t=1393>

---

## A merge B vs. B merge A




---

## rebase vs. merge





REF:  
<https://youtu.be/LgTf7m5B0xA?t=1756>

---

## 預設的`branch`一定要叫 master 嗎?

不一定。這只是一個慣例。

要不要遵守，就隨意。

REF:  
<https://youtu.be/LgTf7m5B0xA?t=1839>

---

## 合併過的`branch`可不可以砍掉?

可以。

砍合併過的`branch`，不會影響物件的存在。

REF:  
<https://youtu.be/LgTf7m5B0xA?t=1851>

---

## master `branch`可不可以砍掉?

可以。

砍合併過的`branch`，不會影響物件的存在。

REF:  
<https://youtu.be/LgTf7m5B0xA?t=1873>

---

可以砍任何的`branch`，但前提是人(`HEAD`)不能在那個`branch`上，否則要去哪裡?

另外，救`branch`的方法很多。所以，`git`要刪檔，不是那麼簡單。

---

## 關於 `tag` (標籤)

`tag`跟`branch`很像，但`tag`不會隨著`commit`而前進。

---

## `git`做資源回收 (garbage collection)

使用command `git count-objects`，可以看到物件數量跟大小。

使用command `git gc`，強制手動觸發回收機制。

使用command `git count-objects`，可以看到數量變成0。

但其實不是不見，而是被打包。

使用command `tree .git`，可以看到藏在`object/pack/`的資料夾下。


REF:  
<https://youtu.be/LgTf7m5B0xA?t=2042>

---

## 如何新增客製化的`git`指令?


REF:  
<https://youtu.be/LgTf7m5B0xA?t=2086>

---

# 相關連結

git官網:  
<https://git-scm.com/>

YouTube播放清單: Git 程式版本線上課程教學 - 由簡單到難  
<https://www.youtube.com/playlist?list=PL2SrkGHjnWcxw6N1-hg34nWy8UWsFOn5V>

YouTube: 高見龍 - 你知道 Git 是怎麼一回事嗎  
<https://www.youtube.com/watch?v=LgTf7m5B0xA>


YouTube播放清單: 高見龍 - GIT 小教室
<https://www.youtube.com/watch?v=VShhhq_5sMc&list=PLBd8JGCAcUAF2_im__kqZTfEAKnlmfPJy>


YouTube: 高見龍 - 藉由視覺化效果來學習 Git 觀念
<https://www.youtube.com/watch?v=5joXSO4kSCM>
