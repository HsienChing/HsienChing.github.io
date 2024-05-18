---
title: "如何將PDF檔案嵌入HTML頁面，進行顯示?"
date: 2024-05-19 00:30:00 +0800
excerpt: "如何將PDF檔案嵌入HTML頁面，進行顯示? 以下提供並測試幾個方案，並對放在arXiv及GitHub上面的PDF進行測試。"
categories:
  - GitHub Pages
tags:
  - PDF
  - HTML
  - Markdown
  - Post
  - Jekyll
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

如何將PDF檔案嵌入HTML頁面，進行顯示?

以下提供並測試幾個方案，並對放在arXiv及GitHub上面的PDF進行測試。

# 使用 embed

```html
<embed src="https://arxiv.org/pdf/2405.04225" type="application/pdf" width="100%" height="100%">
```

效果:  

對放在arXiv上的PDF，有效。

<embed src="https://arxiv.org/pdf/2405.04225" type="application/pdf" width="100%" height="100%">

<br>

但對放在GitHub的repository上的PDF就沒效了。GitHub拒絕連線。

<embed src="https://github.com/HsienChing/Project-Life-Trace/blob/c4c60a8ed4c9fed098a9e1d330e3276400d168f2/Dataset/H.C.-Chung/E-mails/arXiv-Endorsement/2024-05-07-arXiv-You-now-can-submit-to-eess.SY.pdf" type="application/pdf" width="100%" height="100%">

<br>

# 使用 iframe

```html
<iframe src="https://arxiv.org/pdf/2405.04225" style="width:100%; height:100%;" frameborder="0"></iframe>
```

或

```html
<iframe src="https://arxiv.org/pdf/2405.04225" style="width:718px; height:700px;" frameborder="0"></iframe>
```

效果:  

對放在arXiv上的PDF，有效。

<iframe src="https://arxiv.org/pdf/2405.04225" style="width:100%; height:100%;" frameborder="0"></iframe>

<br>

但對放在GitHub的repository上的PDF就沒效了。GitHub拒絕連線。

<iframe src="https://github.com/HsienChing/Project-Life-Trace/blob/c4c60a8ed4c9fed098a9e1d330e3276400d168f2/Dataset/H.C.-Chung/E-mails/arXiv-Endorsement/2024-05-07-arXiv-You-now-can-submit-to-eess.SY.pdf" style="width:100%; height:100%;" frameborder="0"></iframe>

<br>

# 使用 object

```html
<object width="400" height="500" type="application/pdf" data="/my_pdf.pdf?#zoom=85&scrollbar=0&toolbar=0&navpanes=0">
    <p>Insert your error message here, if the PDF cannot be displayed.</p>
</object>
```

效果:  

對放在arXiv上的PDF，有效。

<object width="100%" height="100%" type="application/pdf" data="https://arxiv.org/pdf/2405.04225">
    <p>Insert your error message here, if the PDF cannot be displayed.</p>
</object>

<br>

但對放在GitHub的repository上的PDF就沒效了。GitHub拒絕連線。

<object width="100%" height="100%" type="application/pdf" data="https://github.com/HsienChing/Project-Life-Trace/blob/c4c60a8ed4c9fed098a9e1d330e3276400d168f2/Dataset/H.C.-Chung/E-mails/arXiv-Endorsement/2024-05-07-arXiv-You-now-can-submit-to-eess.SY.pdf">
    <p>The PDF cannot be displayed. (出現這行字，表示PDF無法顯示)</p>
</object>

<br>


# 相關連結

Stackoverflow: [Recommended way to embed PDF in HTML?](<https://stackoverflow.com/questions/291813/recommended-way-to-embed-pdf-in-html>)

