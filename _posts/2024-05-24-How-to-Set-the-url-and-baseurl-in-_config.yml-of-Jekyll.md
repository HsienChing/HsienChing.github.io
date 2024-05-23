---
title: "如何設定Jekyll的_config.yml檔中的url及baseurl"
date: 2024-05-24 00:15:00 +0800
excerpt: ""
categories:
  - GitHub Pages
tags:
  - HTML
  - Markdown
  - Post
  - Jekyll
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

當把網頁架設在GitHub上，使用GitHub Pages來顯示靜態網頁時。如何設定Jekyll的_config.yml檔中的url及baseurl，就成了一個問題。

# 遭遇問題與嘗試解法

原本網站是架在`https://[username].github.io`這個網域上。所以Jekyll的_config.yml檔，僅需設定`url`，不用設定`baseurl`。

```yml
url                      : "https://[username].github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : # the subpath of your site, e.g. "/blog"
repository               : "[username]/[username].github.io" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

但另外開一個叫`ProjectSite`的專案(就假設這個專案名稱叫`ProjectSite`)，並設定其為GitHub Pages後，GitHub預設會將這個GitHub Pages架在`https://[username].github.io/ProjectSite`下面。所以Jekyll的_config.yml檔，就需要設定`url`及`baseurl`。設置如下。

```yml
url                      : "https://[username].github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : "/ProjectSite" # the subpath of your site, e.g. "/blog"
repository               : "[username]/ProjectSite" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

# 案例

Minimal MistakesA Jekyll theme這個站點，網址為<https://mmistakes.github.io/minimal-mistakes/>

其Jekyll的_config.yml檔，裡面的`url`及`baseurl`設置如下:

```yml
url                      : "https://mmistakes.github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : "/minimal-mistakes" # the subpath of your site, e.g. "/blog"
repository               : "mmistakes/minimal-mistakes" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

# 詳細解釋url及baseurl

網友Parker對url及baseurl有詳細解釋，可參考他的說明。

[Clearing Up Confusion Around baseurl -- Again](https://byparker.com/blog/2014/clearing-up-confusion-around-baseurl/)

# 相關連結

[Static files url does not support baseurl #2553](https://github.com/mmistakes/minimal-mistakes/discussions/2553)

[add baseurl error #3110](https://github.com/mmistakes/minimal-mistakes/discussions/3110)

[Baseurl and url in config doesn't work for Jekyll on Github](https://stackoverflow.com/questions/75115656/baseurl-and-url-in-config-doesnt-work-for-jekyll-on-github)
