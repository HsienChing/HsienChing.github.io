---
title: "縮小樹莓派作業系統使用的硬碟空間，並製作容量較小的img檔 <br> Reduce the hard disk space used by the Raspberry Pi operating system and create an img file with a smaller capacity"
date: 2025-03-19 23:30:00 +0800
excerpt: "將樹莓派的 8 GB 的 OS image 檔寫入 128 GB 的 micro SD 後，使用一陣子後，再來製作這個 micro SD 卡的 img 檔，發現檔案大小就是這張 micro SD 卡的容量。明明系統只有使用 8 GB 的空間，怎麼 img 檔變成 128 GB 這麼大。該如何瘦身?"
categories:
  - Linux
  - RaspberryPi
  - Computer
#tags:
#  - GitHub Pages template
#  - Markdown
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

# 1. 問題描述

將樹莓派的 8 GB 的 OS image 檔寫入 128 GB 的 micro SD 後，使用一陣子後，再來製作這個 micro SD 卡的 img 檔，發現檔案大小就是這張 micro SD 卡的容量。

明明系統只有使用 8 GB 的空間，怎麼 img 檔變成 128 GB 這麼大。

該如何瘦身?

NOTE: 樹莓派的 OS image 檔，有太多種了，一般而言，容量大小從2到16 GB都有。

NOTE: 如果樹莓派的 Linux OS 系統已經經過 raspi-config 的空間擴展，將使用空間擴展到整片 micro SD 卡，請看本Po文。如果樹莓派的 Linux OS 系統沒有經過空間擴展，只是想單純將小容量的作業系統製作 img 檔，請看此Po文 ([不改變樹莓派作業系統使用的硬碟空間，並製作容量較小的img檔]({{ site.baseurl }}{% link _posts/2025-03-20-Modify-img-size-of-linux-OS-Part-2.md %}))。

# 2. 處理方式

## 2.1. 軟硬體環境

此方法須在Linux系統上面做。

由於獻慶手頭上沒有Linux機，因此在Windows的PC上使用VMware來虛擬Linux機來處理。

如果電腦直接安裝Linux系統，則可以直接做。

NOTE: 進行瘦身流程前，請先將 micro SD 卡先備份起來。方便做錯流程時，重新再來一次。

NOTE: 以下步驟，有些是可以跳過不做的，前提是要相當清楚想要修改容量大小的 "儲存空間位置"。很多步驟其實是為了要確認這個 "儲存空間位置"。在搞不清楚的狀況下，img檔沒瘦身到，反倒很可能一個低階指令就會把自己的電腦給搞爛掉了。要重灌電腦。

## 2.2. Mount SD卡

要讓Linux看得到SD卡 (可用VMware，新增硬體達成)

當使用VMware時，Linux可能已經自動mount上SD卡的硬體，若沒有，那就要mount一下，才能看到使用狀況。

## 2.3. 執行指令 `df`

會看到下面的資訊

```bash
osboxes@osboxes:~$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
tmpfs            1633232     2060   1631172   1% /run
/dev/sda1      226577932 12302752 202692872   6% /
tmpfs            8166160        0   8166160   0% /dev/shm
tmpfs               5120        4      5116   1% /run/lock
/dev/sda5      277647428 27036216 236434700  11% /home
/dev/sda2         257848   180968     56384  77% /boot
tmpfs            1633232      100   1633132   1% /run/user/1000
/dev/sdb2      122571784 15644084 100679076  14% /media/osboxes/rootfs
```

主要要看 `/dev/sdb2` 的資料，就是 mirco SD 卡的資料。共有122571784個1K-blocks

換算容量得到 116.89 GB  
122571784 kB / (1024 kB/MB) / (1024 MB/GB)  
= 122571784/1024/1024  
= 116.89 (GB)

## 2.4. Unmount SD卡

接下來的指令，都是要在 SD 卡沒有 mount 進去的狀態下執行。

由於剛剛進行了 mount SD 卡的動作，現在需要 unmount SD 卡。

## 2.5. 執行指令 `sudo fdisk -l`

查看目前磁碟使用狀態。執行指令後，會出現一堆資料，要挑出 `/dev/sdb2` 相關的資料。

會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo fdisk -l
Device     Boot   Start       End   Sectors   Size Id Type
/dev/sdb1          8192   1056767   1048576   512M  c W95 FAT32 (LBA)
/dev/sdb2       1056768 250347519 249290752 118.9G 83 Linux
```

可看到 micro SD 卡切成了兩槽 `sdb1` 及 `sdb2`

`sdb1` 從第8192個~第1056767個 sector，共使用1048576 sectors。

`sdb2` 從第1056768個~第250347519個 sector，共使用249290752 sectors。

每個sector為512bytes，換算成MB

| Device | Start (MB) | End (MB) | Size (MB) |
| :---: | ---: | ---: | ---: |
| sdb1 | 4 | 515.99 | 512 |
| sdb2 | 516 | 122239.99 | 121724 |

計算方式:  
OOO sector x (512 Byte/sector) / (1024 Byte/kB) / (1024 kB/MB)

## 2.6. 執行指令 `sudo e2fsck -f /dev/sdb2`

檢查 micro SD 卡文件系統完整性。若遇到有提示要修復，請輸入 `y`

到最後，會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo e2fsck -f /dev/sdb2
e2fsck 1.46.5 (30-Dec-2021)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
rootfs: 292718/7790592 files (0.2% non-contiguous), 4429419/31161344 blocks
```

表示OK。

## 2.7. 執行指令 `sudo resize2fs -M /dev/sdb2 `

縮小文件系統 (把文件從檔案系統前面排過來，免得縮小檔案系統時，把文件幹掉了)

會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo resize2fs -M /dev/sdb2 
resize2fs 1.46.5 (30-Dec-2021)
Resizing the filesystem on /dev/sdb2 to 4475657 (4k) blocks.
The filesystem on /dev/sdb2 is now 4475657 (4k) blocks long.
```

這表示已經把檔案放到 4475657 blocks 容量的空間下，而每個 block 的大小為 4 kB。

可以換算為 17.07 GB。表示 `sdb2` 的使用空間已經由上百GB縮小到 17 GB左右。

計算方式:  
4475657*4=17902628 (kB)  
17902628/1024/1024=17.07 (GB)

## 2.8. 執行指令 `sudo fdisk /dev/sdb`

看一次SD卡分區狀況

執行指令 `sudo fdisk /dev/sdb` 的過程中輸入p看分區狀況

會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo fdisk /dev/sdb

Welcome to fdisk (util-linux 2.37.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/sdb: 119.38 GiB, 128177930240 bytes, 250347520 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x25d40812

Device     Boot   Start       End   Sectors   Size Id Type
/dev/sdb1          8192   1056767   1048576   512M  c W95 FAT32 (LBA)
/dev/sdb2       1056768 250347519 249290752 118.9G 83 Linux
```

到這裡，指令先不要關掉，我們要先計算一些數值。

## 2.9. 計算 `sdb2` 的結束sector

根據執行指令 `sudo fdisk /dev/sdb` 得到的資料，來計算 `sdb2` 的結束sector

可看到SD卡的 `sdb2` (Linux系统) 開始sector是1056768，结束sector是250347519。每一個sector為512byte。

之前用 `resize2fs` 調整文件系統為 17.07 GB (17902628 kB)  
這裡手動計算結束sector  
以17.1GB來進行計算(17.1 GB > 17.07 GB 較安全)

17.1 GB所需sector為  
17.1 GB * 1024 (MB/GB) * 1024 (kB/MB) * 1024 (B/kB) * (1/512) (sector/B)   
= 17.1 * 1024 * 1024 * 1024 / 512  
= 35861299.2 sectors  
~ 35861300 sectors (無條件進位)

可算得 `sdb2` 的結束sector為  
= `sdb2` 開始的sector + 17.1 GB所需sector  
= 1056768 + 35861300 = 36918068

## 2.10. 繼續執行指令 `sudo fdisk /dev/sdb`

設定fdisk

就是要刪除較大的 `sdb2`，再重新建立較小的 `sdb2`

1. 輸入d, 刪除分區
2. 輸入2
3. 輸入p，查看分區情况，可以看到少了分區2
4. 輸入n，新建分區
5. 輸入p，選擇分區類型為primary
6. 輸入2，設置分區號為2
7. 輸入分區開始位置:  1056768，抄原來的開始sector位置
8. Do you want to remove the signature? [Y]es/[N]o: n
9. 輸入分區結束位置: 35861300，结束sector是手算出來的
10. 輸入p，再次查看分區情况
11. 輸入w，保存設置並退出

會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo fdisk /dev/sdb

Welcome to fdisk (util-linux 2.37.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/sdb: 119.38 GiB, 128177930240 bytes, 250347520 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x25d40812

Device     Boot   Start       End   Sectors   Size Id Type
/dev/sdb1          8192   1056767   1048576   512M  c W95 FAT32 (LBA)
/dev/sdb2       1056768 250347519 249290752 118.9G 83 Linux

Command (m for help): d
Partition number (1,2, default 2): 2

Partition 2 has been deleted.

Command (m for help): p
Disk /dev/sdb: 119.38 GiB, 128177930240 bytes, 250347520 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x25d40812

Device     Boot Start     End Sectors  Size Id Type
/dev/sdb1        8192 1056767 1048576  512M  c W95 FAT32 (LBA)

Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2): 2
First sector (2048-250347519, default 2048): 1056768
Last sector, +/-sectors or +/-size{K,M,G,T,P} (1056768-250347519, default 250347519): 36918068

Created a new partition 2 of type 'Linux' and of size 17.1 GiB.
Partition #2 contains a ext4 signature.

Do you want to remove the signature? [Y]es/[N]o: n

Command (m for help): p

Disk /dev/sdb: 119.38 GiB, 128177930240 bytes, 250347520 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x25d40812

Device     Boot   Start      End  Sectors  Size Id Type
/dev/sdb1          8192  1056767  1048576  512M  c W95 FAT32 (LBA)
/dev/sdb2       1056768 36918068 35861301 17.1G 83 Linux

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

## 2.11. 執行指令 `sudo e2fsck -f /dev/sdb2`

檢查 `sdb2` 的文件系統完整性。

若檔案不完整，
則須重新fdisk，
調整結束sector大小，
直到檔案完整為止。

會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo e2fsck -f /dev/sdb2
e2fsck 1.46.5 (30-Dec-2021)
Pass 1: Checking inodes, blocks, and sizes
Inode 1066448 extent tree (at level 1) could be narrower.  Optimize<y>? yes
Pass 1E: Optimizing extent trees
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information

rootfs: ***** FILE SYSTEM WAS MODIFIED *****
rootfs: 292718/1122304 files (0.3% non-contiguous), 4007571/4475657 blocks
```

在上面的過程中，`e2fsck` 程式指出，還可以最佳化，於是就按 `y`

再重新執行一次 `sudo e2fsck -f /dev/sdb2`

會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo e2fsck -f /dev/sdb2
e2fsck 1.46.5 (30-Dec-2021)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
rootfs: 292718/1122304 files (0.3% non-contiguous), 4007571/4475657 blocks
```

表示沒有問題，到這裡檔案分區已經瘦身完成。可以往下一步走了。

## 2.12. 執行指令 `dd`

再來就要製作img檔

執行指令 `sudo dd if=/dev/sdb of=~/Backup.img count=36918069 status=progress`

`if` 指的是 input filepath  
`of` 指的是 output filepath  
`count` 值輸入之前計算的 "結束sector" +1 (確保比  結束sector  還大)  
36918068 + 1 = 36918069  
`status=progress` 指的是 "要顯示過程"，這樣才會知道過程進行到哪裡。

會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo dd if=/dev/sdb of=~/Backup.img count=36918069 status=progress
18883998720 bytes (19 GB, 18 GiB) copied, 753 s, 25.1 MB/s 
36918069+0 records in
36918069+0 records out
18902051328 bytes (19 GB, 18 GiB) copied, 755.301 s, 25.0 MB/s
```

`dd` 指令製作 img 檔，花費約 13 分鐘。  
755.301 s / (60 s/min) = 12.59 min ~ 13 min

做到這裡，img檔已經製作完成。

PS: `dd` 的原意為 data duplicator，但由於 `dd` 屬於較低階的資料處理工具，通常都會以管理者(root)權限來執行，如果稍有不慎，也很容易造成嚴重的後果(例如: 整顆硬碟的資料不見等等)，所以有些人也把 `dd` 取名為 data destroyer

# 3. 測試img檔方式

## 3.1. 將img檔燒錄進一片容量為32 GB的micro SD卡。插入樹莓派，進行開機測試。

由於img檔大小為17.6 GB，使用一片32 GB的micro SD卡即可燒入進去。

就用這片小容量的micro SD卡插入樹莓派，進行開機測試。

## 3.2. 執行指令 `lsblk`

進入樹莓派後，執行指令 `lsblk`。

會看到下面的資訊

```bash
rpi5@rpi5ess:~ $ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
mmcblk0     179:0    0 29.8G  0 disk 
├─mmcblk0p1 179:1    0  512M  0 part /boot/firmware
└─mmcblk0p2 179:2    0 17.1G  0 part /
```

這表示micro SD卡名字為 `mmcblk0`，裡面有兩個區塊，分別為512 MB的 `mmcblk0p1`、及17.1 GB的 `mmcblk0p2`。

17.1 GB的 `mmcblk0p2` 區塊就是我們之前在Linux系統中瘦身成功的硬碟區塊 `sdb2`。

## 3.2. 執行指令 `df`

會看到下面的資訊

```bash
rpi5@rpi5ess:~ $ df
Filesystem     1K-blocks     Used Available Use% Mounted on
udev             3947712        0   3947712   0% /dev
tmpfs             824560     6064    818496   1% /run
/dev/mmcblk0p2  17516700 15528928   1076264  94% /
tmpfs            4122784      336   4122448   1% /dev/shm
tmpfs               5120       48      5072   1% /run/lock
/dev/mmcblk0p1    522230    77132    445098  15% /boot/firmware
tmpfs             824544      192    824352   1% /run/user/1000
```

主要要看 `mmcblk0p2` 區塊的資料。佔有17516700個1K-blocks、已使用15528928個1K-blocks、仍有1076264個1K-blocks可用、使用率94%。

來進行點小計算，好對這些數字有些感覺。

1. 計算使用率 94%

已使用15528928個1K-blocks、仍有1076264個1K-blocks可用，表示全部共有16605192個1K-blocks可用。

15528928 + 1076264 = 16605192

將已使用的1K-blocks數量除以全部的1K-blocks數量，得到93.51%，四捨五入後會得到94%。

15528928 / 16605192 = 93.51% 

如果將已使用的1K-blocks數量除以表中的那個1K-blocks的數值17516700，會得到88.65%，反而得不到94%。

15528928 / 17516700 = 88.65%

2. 實際使用的空間，會比較小

在 `lsblk` 指令下，得到 `mmcblk0p2` 的大小是 17.1 GB。符合我們在Linux進行瘦身的大小。

但在 `df` 指令下，我們看到 `mmcblk0p2` 區塊的資料。佔有17516700個1K-blocks，而其中的16605192個1K-blocks是用來儲存檔案用。

換算一下，這兩個值與17.1 GB的比例，我們會感受到，實際能用的空間一值在縮小。

17516700 / 1024 / 1024 = 16.71 (GB)  
16.71 / 17.1 = 97.72%

16605192 / 1024 / 1024 = 15.84 (GB)  
15.84 / 17.1 = 92.63%


NOTE: 為何不用指令 `df -h` 得到的數值來計算?  

雖然 `df -h` 會自動換算成 MB, GB等單位，但其小數位數不足，不是那麼容易判別數值間的關係。

以下列出執行指令 `df -h` 後，會看到的資訊

```bash
rpi5@rpi5ess:~ $ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            3.8G     0  3.8G   0% /dev
tmpfs           806M  6.0M  800M   1% /run
/dev/mmcblk0p2   17G   15G  1.1G  94% /
tmpfs           4.0G  336K  4.0G   1% /dev/shm
tmpfs           5.0M   48K  5.0M   1% /run/lock
/dev/mmcblk0p1  510M   76M  435M  15% /boot/firmware
tmpfs           806M  192K  806M   1% /run/user/1000
```

## 3.3. 執行指令 `sudo apt-get update` 及 `sudo apt-get upgrade`

更新Linux作業系統。

如果有問題，也會報錯。

這時候就會突然了解到，為何不要瘦身太極限。如果我們將 `mmcblk0p2` 這個區塊瘦到極限，那就沒有空間去運作Linux作業系統，甚至是update。

## 3.4. 執行其他應用程式

執行一些自己平常在使用的應用程式，看看是否都運作正常。

執行不了，就是 img 檔製作GG了，那就重新來吧!

獻慶已經遭遇過了! 只是很消耗時間!

# 4. 瘦身比例計算

整張 128 GB 的 micro SD 卡，其 img 檔大小為 119 GB   
瘦身後的 micro SD 卡，其 img 檔大小為 17.6 GB

瘦身比例為 15%  
17.6 GB / 119 GB = 14.79% ~ 15%

效果很好!

NOTE: 瘦身比例參考用就好，畢竟使用的 micro SD 卡，容量越大，瘦身比例就會越大。

# 5. 完工

img檔請找個合適的位置存放。

完工!

# 6. 感想

1.  
其實整個第2點的處理方式，可能很多人看了就暈頭轉向，很複雜的感覺。

這也是獻慶將其整理起來的原因。有需要時，直接參考就行。

一步一步來處理，就會達到我們要的結果。

確實，這整個流程，有一定的複雜性。這也就是樹莓派目前還沒有發展一鍵瘦身功能的原因之一。

不過，為了節省電腦裡面的儲存容量，學一學這個流程，還是很划算的。而且還可以認識很多Linux的指令跟功能。並且對儲存系統架構有一定的了解。

2.  
其實這整個流程，就是早期的電腦技術，縮小Linux作業系統使用的硬碟空間。

只是現在樹莓派比較流行，因此就針對樹莓派進行特化的流程整理。

3.  
獻慶目前沒有找到更簡單的方式，那就暫時先這樣了!

# 相關連結

die.net: [resize2fs(8) - Linux man page](<https://linux.die.net/man/8/resize2fs>)

Stack Exchange: [What does resize2fs command do in Linux](<https://unix.stackexchange.com/questions/231598/what-does-resize2fs-command-do-in-linux>)

man7.org: [dd(1) — Linux manual page](<https://man7.org/linux/man-pages/man1/dd.1.html>)
