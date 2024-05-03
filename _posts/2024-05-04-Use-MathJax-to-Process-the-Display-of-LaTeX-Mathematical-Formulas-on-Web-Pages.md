---
title: "[問題處理] 使用MathJax處理網頁上的LaTeX數學公式顯示"
date: 2024-05-04 00:35:00 +0800
excerpt: ""
categories: 
  - Problem
  - 問題
  - GitHub Pages
tags:
  - Solving issues
  - 處理問題
  - GitHub
  - Jekyll
  - MathJax
  - LaTeX
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

2024-04-12 星期五

花了一堆時間在處理Markdown裡面的$LaTeX$代碼顯示。

發現使用`MathJax`這個開源的JavaScript顯示引擎，是個不錯的處理方式。

但才剛用，就踢鐵板了。

在搞了半天後，才發現`MathJax`在每個作業環境下的處理方式都不太一樣。

在搞不清楚作業環境的狀況下，處理方式又混用，那就進入一團亂的狀態，又浪費一堆時間。

# 不同的作業環境

一開始就是連作業環境都搞不清楚，才不斷失誤。

後來才開始慢慢搞清楚。

本次共有三個作業環境。

1. 本地電腦安裝的Jekyll平台。
2. GitHub平台。
3. GitHub Pages中的Jekyll平台。

以下將整理好的，安裝方式介紹。

## 本地電腦安裝的Jekyll平台

針對本地電腦安裝的Jekyll平台，就是在自己電腦或筆電上面安裝的Jekyll平台。

使用Jekyll的`MathJax`外掛(plugin)就可以簡單處理。GitHub上面可以查到一堆，這裡就不列舉。

獻慶自己測試過這款:  
jekyll-spaceship: <https://github.com/jeffreytse/jekyll-spaceship>  
使用方式請自行見官網說明。  
基本上就是修改`Gemfile`跟`_config.yml`兩個檔案，然後`bundle install`，再run Jekyll。

經過測試，可正常顯示。

GOOD! ^_^

NOTE: 這款Jekyll外掛在本機電腦上使用很順暢。但若用在GitHub Pages中的Jekyll平台，在網站編譯過程中，會出現`build warning`，警告訊息為"The github-pages gem can't satisfy your Gemfile's dependencies."，然後$LaTeX$公式就會以"原始碼"顯示，而不會看到漂亮的數學式。冏!

## GitHub平台

在GitHub平台中的說明文件，有提到

> To enable clear communication of mathematical expressions, GitHub supports LaTeX formatted math within Markdown.
>
> GitHub Docs - Writing mathematical expressions  
> <https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions>

意思就是，GitHub平台原生支援$LaTeX$的數學公式顯示。

那就不用再安裝`MathJax`，因為GitHub平台已經處理好。

經過獻慶放檔案上去測試，發現GitHub平台不需要經過任何處理，確實會自動把$LaTeX$數學公式正常顯示出來。

GOOD! ^_^

## GitHub Pages中的Jekyll平台

使用Jekyll外掛在本機電腦上跑`MathJax`很順暢。但若用在GitHub Pages中的Jekyll平台，則在網站編譯過程中，出現警告訊息為"The github-pages gem can't satisfy your Gemfile's dependencies."，然後$LaTeX$公式就會以"原始碼"顯示，而不會看到漂亮的數學式。冏!

沒辦法直接用Jekyll外掛的原因是，GitHub Pages有自己的外掛白名單(<https://pages.github.com/versions/>)。而`jekyll-spaceship`這款外掛，就沒有在白名單上。

為了這個問題，就得開始另外找答案。

整理後的方法如下:

由於獻慶是使用Minimal Mistakes提供的Jekyll環境。以下的方法是用於Minimal Mistakes得Jekyll環境。
(如果不是使用，就要參考Jekyll的說明檔，<https://jekyllrb.com/docs/configuration/>)

**Step. 1. 設定Markdown引擎為kramdown**

檢查`_config.yml`檔中的`markdown`選項，若不是`kramdown`，請改為`kramdown`。

```yml
markdown: kramdown
```

**Step. 2. 增加一段MathJax的JavaScript程式碼**

有幾個地方可以加入MathJax的JavaScript程式碼:  
1. `/_includes/scripts.html` (獻慶測試後，OK。直接貼到檔案尾端就行，記得要存檔。)
2. `/_includes/head/custom.html` (獻慶測試後，OK。直接貼到第一跟第二兩段註解中間。)
3. `/_includes/footer/custom.html` (獻慶測試後，OK。直接貼到頭尾兩段註解中間。)

MathJax的JavaScript程式碼附在下面。

貼完JavaScript程式碼後，記得要存檔。

**Step. 3. 重新載入有寫$LaTeX$數學式的頁面**

重新載入頁面後，$LaTeX$數學式應該就會正常顯示。

本文也提供一個案例，放在下面當參考。

**整理過的MathJax的JavaScript程式碼:**

獻慶最後是選擇將程式碼貼在`/_includes/head/custom.html`中。

```javascript
<!-- Mathjax Support -->
<script type="text/javascript" async
	src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.9/latest.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/x-mathjax-config">
   MathJax.Hub.Config({
     extensions: ["tex2jax.js"],
     jax: ["input/TeX", "output/HTML-CSS"],
     tex2jax: {
       inlineMath: [ ['$','$'], ["\\(","\\)"] ],
       displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
       processEscapes: true
     },
     "HTML-CSS": { availableFonts: ["TeX"] }
   });
</script>
```

NOTE: `MathJax`在GitHub上的來源庫裡面，寫著`release-v2 v2.7.9`，於是獻慶就測試將MathJax的JavaScript程式碼裡面的版本改成2.7.9，經過測試，OK。(未來有新版的時候，可以自行修改。)

GitHub: MathJax (Source Repository)  
<https://github.com/mathjax/MathJax-src>

GOOD! ^_^

## 可行方式整理

3個作業環境，2種處理方式。搞不清楚在幹什麼的話，真的會"搞死人"!

這裡針對全部6種排列組合($3 \times 2 = 6$)，整理一個表格，方便參考。

|              | 本地電腦安裝的 <br> Jekyll平台 |  GitHub平台 | GitHub Pages中的 <br> Jekyll平台 |
| ------------ | ---------------------------- | ----------- | ------------------------ |     
| 使用Jekyll外掛 | OK | 原生支援`MathJax` <br> 無需使用 | 可能會有問題 <br> GitHub Pages有外掛白名單限制 |
| 使用JavaScript | OK | 原生支援`MathJax` <br> 無需使用 | OK |

# 成功案例

成功處理完`MathJax`的問題後，就可以順利顯示$LaTeX$的數學公式顯示。

以下，獻慶就來寫個，物理學的電磁學裡面，那個有名的馬克斯威爾方程式(Maxwell's equations)。

原始碼:
```markdown
$$
\begin{align}
\nabla \cdot  \mathbf{E} &=  \frac{\rho}{\varepsilon_0} \\ 
\nabla \cdot  \mathbf{B} &=  0 \\ 
\nabla \times \mathbf{E} &= -\frac{\partial \mathbf{B}}{\partial t} \\ 
\nabla \times \mathbf{B} &=  \mu_0 \left( \mathbf{J} + \varepsilon_0 \frac{\partial \mathbf{E}}{\partial t} \right)
\tag{1.1}
\end{align}
$$
```

顯示:  
$$
\begin{align}
\nabla \cdot  \mathbf{E} &=  \frac{\rho}{\varepsilon_0} \\ 
\nabla \cdot  \mathbf{B} &=  0 \\ 
\nabla \times \mathbf{E} &= -\frac{\partial \mathbf{B}}{\partial t} \\ 
\nabla \times \mathbf{B} &=  \mu_0 \left( \mathbf{J} + \varepsilon_0 \frac{\partial \mathbf{E}}{\partial t} \right)
\tag{1.1}
\end{align}
$$

漂亮!

GOOD! ^_^

# 關於MathJax

> `MathJax` is an open-source JavaScript display engine for $LaTeX$, MathML, and AsciiMath notation that works in all modern browsers.
>
> GitHub: MathJax (Source Repository)  
> <https://github.com/mathjax/MathJax-src>

See <http://www.mathjax.org/> for additional details about MathJax, and <https://docs.mathjax.org> for the MathJax documentation.

`MathJax` logo:  
![](https://www.mathjax.org/badge/mj-logo.svg){:height="30%" width="30%"}

# 相關連結

Minjeong Choi: How to use Latex(MathJax) on Minimal Mistakes Github blogs  
<https://choimon.github.io/blog/mathjax-for-minimalmistakes-githubpage/>

Singyuan's Blog: Add LaTeX to Minimal Mistake Jekyll    
<https://singyuan.github.io/posts/mathjax/add_tex/>

Janmeppe.com: How to add Latex to Minimal Mistakes  
<https://www.janmeppe.com/blog/How-to-add-mathjax-to-minimal-mistakes/>

GitHub community: How to display Math on Github-Pages? #22525  
<https://github.com/orgs/community/discussions/22525>

Wikipedia: Maxwell's equations  
<https://en.wikipedia.org/wiki/Maxwell%27s_equations>
