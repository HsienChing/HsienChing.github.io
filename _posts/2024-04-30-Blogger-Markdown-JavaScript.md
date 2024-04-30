---
title: "在blogger上使用Markdown，並貼上漂亮code的方法。但並不是很好用。"
date: 2024-0-30 10:10:00 +0800
excerpt: "尋找在blogger上使用Markdown，並貼上漂亮code的方法。測試完後，發現並不是很好用。要好好貼程式碼，還是來GitHub吧! ^_^"
categories:
  - Blogger
tags:
  - Markdown
  - JavaScript
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

# 找尋方法並測試

REF: 在 blogger 貼漂亮 code 的方法（使用 Markdown 和 prettyprint）  
<https://etrex.blogspot.com/2017/03/blogger-code-Markdown-prettyprint.html>

測試後，發現主要問題:  
1. 超連結消失。使用Markdown語法`<>`標注的超連結會消失。
2. 超連結，以純文字顯示。
3. 表格無法顯示。
4. 爆圖問題。使用`Simple Literate theme`會爆圖，就是圖面超過Po文寬度，炸到框外去了。`Contempo theme`就不會爆。

REF: 在 Blogger 中使用 Markdown  
<https://blog.imfing.com/2020/04/Markdown-in-blogger/>

測試後，發現主要問題:  
1. 超連結消失。使用Markdown語法`<>`標注的超連結會消失。(有解決)
2. 超連結，以純文字顯示。(有解決)
3. 表格無法顯示。
4. 爆圖問題。使用`Simple Literate theme`會爆圖，就是圖面超過Po文寬度，炸到框外去了。`Contempo theme`就不會爆。

第1個主要問題。改寫法就好，不要使用Markdown語法`<>`標注超連結就好。

第4個主要問題。改用Contempo系列主題後，就解決了。

# 轉換原理

要在 blogger 上使用 Markdown 其實很簡單，主要概念是用 JavaScript 將 Markdown 的文章內容抓出來，然後用網路上的 JavaScript 函式庫 showdown 將 Markdown 文章轉為 html ，然後再更新回去。

程式相當簡單，接著在寫 blog 時只需要注意兩件事:

- 在要用 Markdown 編寫的文章最開頭放一行 `markdown` 字串讓 script 知道這一篇文章是用 Markdown 寫的，需要做轉換
- 編寫文章時要用 HTML 的模式編寫

上面的程式碼區塊除了 Markdown 的轉換之外，也引用了 Google 的 prettify 函式庫來做程式碼的上色。

REF:  
Markdown on Google blogger  
<https://lausai360.blogspot.com/2018/11>

# 經過整理後，最後貼上去Blogger的JavaScript

目前在Blogger中使用這個版本的JavaScript。

對貼上程式碼的支持度還可以。

```javascript
// The > will replaced to > when using innerHTML, so replace it back to >
// for showdown to render blockquote
function blockquote(str) {
    return str.replace(/\n>/g,'\n>') ;
}
 
var converter = new showdown.Converter(showdownOption);
var posts = document.querySelectorAll(".post-body");
var s = "";
Array.prototype.forEach.call(posts, function(el, i) {
    var idx = el.innerHTML.indexOf("Markdown");
    if(idx != -1 && idx <= 1) {
        el.innerHTML = converter.makeHtml(blockquote(el.innerHTML).replace("Markdown",""));
        var pres = el.querySelectorAll("pre");
        for (var i = 0; i < pres.length; i++) {
            pres[i].classList.add("prettyprint");
            pres[i].classList.add("linenums:1");
        }
    }
});
</script>
 
<script src="https://cdn.rawgit.com/google/code-prettify/master/loader/run_prettify.js">
</script>
```

獻慶在自己的Blogger上使用該JavaScript，讓Blogger可支援Markdown格式。

範例Po文在這裡，該Po文使用Markdown語法進行寫作。
[[音樂] 曼陀林、烏克麗麗、吉他、魯特琴的 雙弦 (Double string) 複弦 討論](<https://dream-and-creation.blogspot.com/2024/02/Markdown.html>)

# 感想

後來發現，要好好貼程式碼，還是來GitHub吧! ^_^

# 相關連結

GitHub: showdownjs / showdown  
<https://github.com/showdownjs/showdown>

CDN  
You can also use one of several CDNs available:  
cdnjs  
https://cdnjs.cloudflare.com/ajax/libs/showdown/`<version tag>`/showdown.min.js  
Note: replace `<version tag>` with an actual full length version you're interested in e.g. 1.9.0  
可以上去看版本變化，使用最新版本。
