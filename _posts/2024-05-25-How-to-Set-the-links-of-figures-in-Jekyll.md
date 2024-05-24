---
title: "在Jekyll中，如何設定網路連結，讓圖檔可以正常顯示。"
date: 2024-05-25 00:15:00 +0800
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

在Jekyll中，如何設定網路連結，讓圖檔可以正常顯示。

# 1. 結論

## 1.1 使用網站外部圖檔

語法:
```markdown
![alt name](<[absolute link]>)
```

範例:
```markdown
![Classical Guitar labelled english](<https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Classical_Guitar_labelled_english.jpg/1024px-Classical_Guitar_labelled_english.jpg>)
```

此時，圖檔的絕對連結`[absolute link]`為<https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Classical_Guitar_labelled_english.jpg/1024px-Classical_Guitar_labelled_english.jpg>

![Classical Guitar labelled english](<https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Classical_Guitar_labelled_english.jpg/1024px-Classical_Guitar_labelled_english.jpg>)

## 1.2 使用網站內部圖檔

語法:
```markdown
![alt name]({{ site.url }}{{ site.baseurl }}[relative link])
```

範例:
```markdown
![]({{ site.url }}{{ site.baseurl }}/assets/images/2024/20230711_193425_1024px.jpg)
```

其中，圖檔的相對連結`[relative link]`為`/assets/images/2024/This-is-a-Figure-1920px-1080px.jpg`  
`{{ site.url }}`表示使用站點的`url`  
`{{ site.baseurl }}`表示使用站點的`baseurl`

![]({{ site.url }}{{ site.baseurl }}/assets/images/2024/This-is-a-Figure-1920px-1080px.jpg)

使用`{{ site.url }}{{ site.baseurl }}`放在`[relative link]`前面的用途就是，讓整個連結形成絕對連結`[absolute link]`，以規避掉Jekyll無法處理相對連結的顯示問題。讓圖檔可以正常顯示。

NOTE: 關於`url`及`baseurl`的討論，請見之前的Po文，[如何設定Jekyll的_config.yml檔中的url及baseurl](<>)。

# 2. 問題討論

> 尋找解決方案的過程，是很重要的。
> 
> 到底如何達到這個結論，說明這個結論是合理的。並且，這個紀錄下來的過程，在未來重新思考問題時，將非常有幫助。
> 
> 鍾獻慶

既然結論都寫完了，那要討論什麼。

這裡只是秀一下，為了處理這個問題，這幾天獻慶走了什麼冤枉路，遇到啥冏事。

## 2.1 遭遇問題

在Markdown檔案中，插入圖檔的語法是
```markdwon
![alt name]([link])
```

其中`[link]`可以是絕對連結(absolute link)或相對連結(relative link)。

這樣的理解還OK。

當我們把站點架在`https://[username].github.io`上時。就算使用相對連結，也不太會遭遇問題。圖都可以正常顯示。而且為了方便在本機上進行網頁測試，會使用相對連結。

但當我們另外開一個站點，就命名為`ProjectSite`好了，這時候站點的位置會在`https://[username].github.io/ProjectSite`。

但此時，就會發現所有的圖檔都無法顯示。

於是開始尋找方法，進行了以下測試。

## 2.2 測試1

測試`url`及`baseurl`排列組合。

修改Jekyll的_config.yml檔為
```yml
url                      : "https://[username].github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : # the subpath of your site, e.g. "/blog"
repository               : "[username]/ProjectSite" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

結果: 無效。

## 2.3 測試2

測試`url`及`baseurl`排列組合。

修改Jekyll的_config.yml檔為
```yml
url                      : "https://[username].github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : "/ProjectSite" # the subpath of your site, e.g. "/blog"
repository               : "[username]/ProjectSite" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

結果: 無效。

## 2.4 測試3

測試`url`及`baseurl`排列組合。

修改Jekyll的_config.yml檔為
```yml
url                      : "https://[username].github.io/ProjectSite" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : # the subpath of your site, e.g. "/blog"
repository               : "[username]/ProjectSite" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

結果: 無效。

## 2.5 測試4

測試`url`及`baseurl`排列組合。

修改Jekyll的_config.yml檔為
```yml
url                      : "https://[username].github.io/ProjectSite" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : "/ProjectSite" # the subpath of your site, e.g. "/blog"
repository               : "[username]/ProjectSite" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

結果: 無效。

## 2.6 測試5

發現Minimal MistakesA Jekyll theme這個站點，其Jekyll的_config.yml檔，裡面的`https://mmistakes.github.io`前後沒有`"`符號。

修改Jekyll的_config.yml檔為
```yml
url                      : https://[username].github.io # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : "/ProjectSite" # the subpath of your site, e.g. "/blog"
repository               : "[username]/ProjectSite" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

結果: 無效。

## 2.7 測試6

修改theme設定。

修改Jekyll的_config.yml檔為
```yml
# theme                  : "minimal-mistakes-jekyll"
# remote_theme           : "mmistakes/minimal-mistakes"
remote_theme             : "mmistakes/minimal-mistakes@4.24.0"
```

結果: 無效。

## 2.8 測試7

修改theme設定。

修改Jekyll的_config.yml檔為
```yml
# theme                  : "minimal-mistakes-jekyll"
remote_theme             : "mmistakes/minimal-mistakes"
# remote_theme           : "mmistakes/minimal-mistakes@4.24.0"
```

結果: 無效。

## 2.9 測試8。

修改theme設定

修改Jekyll的_config.yml檔為
```yml
theme                    : "minimal-mistakes-jekyll"
# remote_theme           : "mmistakes/minimal-mistakes"
# remote_theme           : "mmistakes/minimal-mistakes@4.24.0"
```

結果: 無效。

## 2.10 測試9

參考Minimal MistakesA Jekyll theme這個站點(網址為<https://mmistakes.github.io/minimal-mistakes/>)，及其GitHub上repository的結構。

Minimal MistakesA Jekyll theme這個站點，其Jekyll的_config.yml檔，裡面的`url`及`baseurl`設置如下:
```yml
url                      : "https://mmistakes.github.io" # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
baseurl                  : "/minimal-mistakes" # the subpath of your site, e.g. "/blog"
repository               : "mmistakes/minimal-mistakes" # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
```

而在<https://mmistakes.github.io>這個位置，是設定成GitHub Pages，但是裡面僅有放一個README.md檔案。表示沒有架站。

在看完這個結構後，獻慶就把自己的`https://[username].github.io`下的Jekyll網站相關檔案，全部刪除。再放上一個README.md檔案。完全模仿其檔案結構。

結果: 無效。

## 2.11 測試10

無意間尋找到資料。

網友Raleigh L.分享他的處理方式。

> The way I got inline images to work on Github Pages:
> 
> * In `config.yml`... (1) set the `url` field to just be the "base" of your URL. This URL should match the pattern `https://<your-github-username>.github.io` (2) set the `baseurl` field to be the name of your repository where this Github Pages repository is built from. This field must start with a forward slash, so `/<repository-name>`
> 
> * In your actual Markdown file, you'll cite the image in-line like this:
> 
> `![alt]({{ site.url }}{{ site.baseurl }}/assets/images/filename.jpg)`

REF:  
<https://stackoverflow.com/questions/75115656/baseurl-and-url-in-config-doesnt-work-for-jekyll-on-github>

獻慶也另外搜尋網友mmistakes的文件檔，裡面也有提到處理方式。這個文件檔，也剛好是上面那個網友的參考來源(真巧 ^_^)。

> The preferred way of using images is placing them in the `/assets/images/` directory and referencing them with an absolute path. Prepending the filename with `{% raw %}{{ site.url }}{{ site.baseurl }}/assets/images/{% endraw %}` will make sure your images display properly in feeds and such.

REF:  
<https://github.com/mmistakes/minimal-mistakes/blob/master/docs/_posts/2010-08-05-post-image-standard.md>

經過測試，這個語法可以順利顯示圖檔。
```markdown
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/filename.jpg)
```

# 3. 延伸思考

不只是圖檔，而是一些網頁內部檔案連結都可以用這個方式。

看來網頁程式碼內容，又要全新大改版了。

# 4. 感想

2024-05-17 星期五 到 2024-05-19 星期日，將近3天的時間都卡在這個問題。冏!

別看是一個小問題，有時候解題思路沒有命中，就會浪費掉很多時間。不過這也是必經之路。

搞到後來，連[如何設定Jekyll的_config.yml檔中的url及baseurl]({{ site.baseurl }}{% link _posts/2024-05-24-How-to-Set-the-url-and-baseurl-in-_config.yml-of-Jekyll.md %})都搞清楚了。

# 5. 相關Po文

[如何設定Jekyll的_config.yml檔中的url及baseurl]({{ site.baseurl }}{% link _posts/2024-05-24-How-to-Set-the-url-and-baseurl-in-_config.yml-of-Jekyll.md %})

# 6. 相關連結

<https://github.com/mmistakes/minimal-mistakes/blob/master/docs/_posts/2010-08-05-post-image-standard.md>  
裡面有詳細的圖檔顯示教學。
