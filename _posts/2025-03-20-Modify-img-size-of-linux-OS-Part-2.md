---
title: "不改變樹莓派作業系統使用的硬碟空間，並製作容量較小的img檔 <br> Do not change the hard disk space used by the Raspberry Pi operating system and create an img file with a smaller capacity"
date: 2025-01-20 23:30:00 +0800
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

<!--
NOTE: 如果樹莓派的 Linux OS 系統已經經過 raspi-config 的空間擴展，將使用空間擴展到整片 micro SD 卡，請看本Po文。如果樹莓派的 Linux OS 系統沒有經過空間擴展，只是想單純將小容量的作業系統製作 img 檔，請看此Po文 ([不改變樹莓派作業系統使用的硬碟空間，並製作容量較小的img檔]({{ site.baseurl }}{% link _posts/2025-03-20-Modify-img-size-of-linux-OS-Part-2.md %}))。
-->
# 2. 處理方式

## 2.1. 軟硬體環境

此方法須在Linux系統上面做。

由於獻慶手頭上沒有Linux機，因此在Windows的PC上使用VMware來虛擬Linux機來處理。

如果電腦直接安裝Linux系統，則可以直接做。

NOTE: 進行瘦身流程前，請先將 micro SD 卡先備份起來。方便做錯流程時，重新再來一次。

NOTE: 以下步驟，有些是可以跳過不做的，前提是要相當清楚想要修改容量大小的 "儲存空間位置"。很多步驟其實是為了要確認這個 "儲存空間位置"。在搞不清楚的狀況下，img檔沒瘦身到，反倒很可能一個低階指令就會把自己的電腦給搞爛掉了。要重灌電腦。

## 2.2. Mount SD卡 (可跳過這2.2.跟2.3.兩步)

要讓Linux看得到SD卡 (可用VMware，新增硬體達成)

當使用VMware時，Linux可能已經自動mount上SD卡的硬體，若沒有，那就要mount一下，才能看到使用狀況。

## 2.3. 執行指令 `df` (可跳過這2.2.跟2.3.兩步)

會看到下面的資訊

```bash
osboxes@osboxes:~$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
tmpfs            1633232     2092   1631140   1% /run
/dev/sda1      226577932 12304564 202691060   6% /
tmpfs            8166144        0   8166144   0% /dev/shm
tmpfs               5120        4      5116   1% /run/lock
/dev/sda2         257848   180968     56384  77% /boot
/dev/sda5      277647428 16895632 246575284   7% /home
tmpfs            1633228       96   1633132   1% /run/user/1000
/dev/sdb2       17516700 15718752    886440  95% /media/osboxes/rootfs
```

主要要看 `/dev/sdb2` 的資料，就是 mirco SD 卡的資料。共有17516700個1K-blocks。使用率為95%。

使用率為95%。確認linux OS檔案幾乎是佔滿儲存空間的狀況。

換算容量得到 16.71 GB  
17516700 kB / (1024 kB/MB) / (1024 MB/GB)  
= 17516700/1024/1024  
= 16.71 (GB)

## 2.4. Unmount SD卡

接下來的指令，都是要在 SD 卡沒有 mount 進去的狀態下執行。

由於剛剛進行了 mount SD 卡的動作，現在需要 unmount SD 卡。

## 2.5 執行指令 `lsblk`

會看到下面的資訊

```bash
osboxes@osboxes:~$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0  63.2M  1 loop /snap/core20/1738
loop1    7:1    0  73.9M  1 loop /snap/core22/1722
loop2    7:2    0  63.7M  1 loop /snap/core20/2434
loop3    7:3    0     4K  1 loop /snap/bare/5
loop4    7:4    0  73.9M  1 loop /snap/core22/1748
loop5    7:5    0 237.5M  1 loop /snap/firefox/2154
loop6    7:6    0 346.3M  1 loop /snap/gnome-3-38-2004/119
loop7    7:7    0 273.7M  1 loop /snap/firefox/5437
loop8    7:8    0   497M  1 loop /snap/gnome-42-2204/141
loop9    7:9    0 349.7M  1 loop /snap/gnome-3-38-2004/143
loop10   7:10   0  81.3M  1 loop /snap/gtk-common-themes/1534
loop11   7:11   0  91.7M  1 loop /snap/gtk-common-themes/1535
loop12   7:12   0  12.2M  1 loop /snap/snap-store/1216
loop13   7:13   0  12.3M  1 loop /snap/snap-store/959
loop14   7:14   0  40.9M  1 loop /snap/snapd/20290
loop15   7:15   0  44.3M  1 loop /snap/snapd/23258
loop16   7:16   0   568K  1 loop /snap/snapd-desktop-integration/253
loop17   7:17   0   452K  1 loop /snap/snapd-desktop-integration/83
sda      8:0    0   500G  0 disk 
├─sda1   8:1    0 220.6G  0 part /var/snap/firefox/common/host-hunspell
│                                /
├─sda2   8:2    0   286M  0 part /boot
├─sda3   8:3    0    95M  0 part 
├─sda4   8:4    0   8.9G  0 part [SWAP]
└─sda5   8:5    0 270.1G  0 part /home
sdb      8:16   0  29.8G  0 disk 
├─sdb1   8:17   0   512M  0 part 
└─sdb2   8:18   0  17.1G  0 part 
sr0     11:0    1  1024M  0 rom  
```

主要要看 `sdb` 的資訊。其中 `sdb1` 大小為 512 MB、`sdb2` 大小為 17.1 GB，全部 `sdb` 的大小為 29.8 GB。

確認 `sdb` 確實是我們要處理的 micro SD 卡。

## 2.6. 執行指令 `sudo fdisk -l`

查看目前磁碟使用狀態。執行指令後，會出現一堆資料，要挑出 `/dev/sdb2` 相關的資料。

會看到下面的資訊

```bash
Device     Boot   Start      End  Sectors  Size Id Type
/dev/sdb1          8192  1056767  1048576  512M  c W95 FAT32 (LBA)
/dev/sdb2       1056768 36918068 35861301 17.1G 83 Linux
```

可看到 micro SD 卡切成了兩槽 `sdb1` 及 `sdb2`

`sdb1` 從第8192個~第1056767個 sector，共使用1048576 sectors。

`sdb2` 從第1056768個~第36918068個 sector，共使用35861301 sectors。

每個sector為512bytes，換算成MB

| Device | Start (MB) | End (MB) | Size (MB) |
| :---: | ---: | ---: | ---: |
| sdb1 | 4 | 515.99 | 512 |
| sdb2 | 516 | 18026.40 | 17510.40 |

計算方式:  
OOO sector x (512 Byte/sector) / (1024 Byte/kB) / (1024 kB/MB)

## 2.7. 執行指令 `sudo e2fsck -f /dev/sdb2`

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
rootfs: 292794/1122304 files (0.3% non-contiguous), 4026170/4475657 blocks
```

表示OK。

## 2.8. 執行指令 `sudo resize2fs -M /dev/sdb2` (直接跳過)

執行指令 `sudo resize2fs -M /dev/sdb2`  
縮小文件系統

請跳過 `resize2fs`，因為sdb2已經夠小了

## 2.9. 執行指令 `sudo fdisk /dev/sdb` (可跳過，需要的數值在2.6.就有出現了)

看一次SD卡分區狀況

執行指令 `sudo fdisk /dev/sdb` 的過程中輸入p看分區狀況

會看到下面的資訊

```bash
osboxes@osboxes:~$ sudo fdisk /dev/sdb

Welcome to fdisk (util-linux 2.37.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/sdb: 29.84 GiB, 32044482560 bytes, 62586880 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x25d40812

Device     Boot   Start      End  Sectors  Size Id Type
/dev/sdb1          8192  1056767  1048576  512M  c W95 FAT32 (LBA)
/dev/sdb2       1056768 36918068 35861301 17.1G 83 Linux
```

到這裡，指令就可以關掉了，我們只要記錄一些數值。

## 2.10. 直接看 `sdb2` 的結束sector

根據執行指令 `sudo fdisk /dev/sdb` (或是 `sudo fdisk -l` )得到的資料，來看出 `sdb2` 的結束sector

可看到SD卡的 `sdb2` (Linux系统) 開始sector是1056768，结束sector是36918068。每一個sector為512byte。

## 2.11. 繼續執行指令 `sudo fdisk /dev/sdb` (直接跳過)

設定fdisk

就是要刪除較大的 `sdb2`，再重新建立較小的 `sdb2` (這次不用這個動作)

## 2.12. 執行指令 `sudo e2fsck -f /dev/sdb2` (直接跳過)

檢查 `sdb2` 的文件系統完整性。

本來就完整。

## 2.13. 執行指令 `dd`

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
18883855872 bytes (19 GB, 18 GiB) copied, 973 s, 19.4 MB/s 
36918069+0 records in
36918069+0 records out
18902051328 bytes (19 GB, 18 GiB) copied, 973.863 s, 19.4 MB/s
```

`dd` 指令製作 img 檔，花費約 16 分鐘。  
973.863 s / (60 s/min) = 16.23 min ~ 16 min

做到這裡，img檔已經製作完成。

PS: `dd` 的原意為 data duplicator，但由於 `dd` 屬於較低階的資料處理工具，通常都會以管理者(root)權限來執行，如果稍有不慎，也很容易造成嚴重的後果(例如: 整顆硬碟的資料不見等等)，所以有些人也把 `dd` 取名為 data destroyer

# 3. 測試img檔方式

基本上就是將img檔燒錄進一片micro SD卡。插入樹莓派，進行開機測試。

進入系統後，執行其他應用程式，測試系統是否可正常運作。

<!--
詳細測試，請參考Po文 ( [縮小樹莓派作業系統使用的硬碟空間，並製作容量較小的img檔]({{ site.baseurl }}{% link _posts/2025-03-20-Modify-img-size-of-linux-OS-Part-1.md %}) )
-->
# 4. 瘦身比例計算

整張 32 GB 的 micro SD 卡，其 img 檔大小為 29.8 GB   
瘦身後的 micro SD 卡，其 img 檔大小為 17.6 GB 

瘦身比例為 59%   
17.6 GB / 29.8 GB = 59.06% ~ 59%

效果很好!

NOTE: 瘦身比例參考用就好，畢竟使用的 micro SD 卡，容量越大，瘦身比例就會越大。

# 5. 完工

img檔請找個合適的位置存放。

完工!

# 6. 感想

如果不調整硬碟空間尺寸，可跳過的步驟很多，也沒有複雜的手動計算，其實節奏很快。
