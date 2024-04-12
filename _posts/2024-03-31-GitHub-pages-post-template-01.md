---
title: "GitHub Pages Post - Markdown File Template 01"
date: 2024-03-31 23:30:00 +0800
excerpt: "A Markdown template file for posts in GitHub pages"
categories: 
  - GitHub pages
tags:
  - GitHub pages template
  - Markdown
  - Post
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

About this template file:

This is a Markdown template file for GitHub pages.

A suggested way to use this file is to see the source file, and then copy-paste the function you need. Try the code and modify the code in your style.

Template link: <https://github.com/HsienChing/HsienChing.github.io/blob/master/_posts/2024-03-31-GitHub-pages-post-template-01.md>

# 1. Section heading (1st-level heading)

Normal text.

## 1.1 Subsection heading (2nd-level heading)

Normal text.

### 1.1.1 Subsubsection heading (3rd-level heading)

Normal text.

#### 1.1.1.1 4th-level heading

Normal text.

##### 1.1.1.1.1 5th-level heading

Normal text.

###### 1.1.1.1.1.1 6th-level heading

Normal text.

# 2. Text styles

## 2.1 Markdown supported

Normal text  
*Italic text*  
_Italic text_  
**Bold text**  
__Bold text__  
***Italic bold text***  
___Italic bold text___  
~~Delete text~~

## 2.2 HTML supported

<u>Underline text</u>

## 2.3 Quoating text

Text that is not a quote

> Text that is a quote

> Hello, World!
>
> Have a nice day!
>
> Hsien-Ching Chung

## 2.4 Color text

Markdown doesn't support color. We can use inline HTML inside Markdown to reach this goal.

Syntax:  
```html
<span style="color:red">some *red* text</span>
```

Example:  
<span style="color:red">some *red* text</span>  
<span style="color:green">some *green* text</span>  
<span style="color:blue">some *blue* text</span>  

# 3. List

## 3.1 Bullet lists

Use `*`, `+`, or `-` to build bullet list.

* Item
* Item
* Item

+ Item
+ Item
+ Item

- Item
- Item
- Item

* Item
+ Item
- Item

## 3.2 Numbered lists

Use `1.` to build numbered list.

1. Item
2. Item
3. Item

1. Item
1. Item
1. Item


1. Item
9. Item
7. Item

Enter a sentence to terminate the continuing number.

1. Item
9. Item
7. Item

## 3.3 List cascade

1. Item
    - aaa
    - bbb
    - ccc
2. Item
    - ddd
    - eee

## 3.4 Task lists

To mark a task as complete, use [x].

- [x] Task 1
- [ ] Task 2
- [ ] Task 3

# 4. Code renderer

Normal text.

## 4.1 Markdown supported

Markdown offers support for code snippets:

```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

## 4.2 Jekyll supported

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

# 5. Hyperlinks

## 5.1 Inline-style links

### 5.1.1 

Use `<` and `>`

Link: https://hsienching.github.io/

Link: <https://hsienching.github.io/>

### 5.1.2

This is [My GitHub pages](<https://hsienching.github.io/> "Title") inline link.

[This link](<https://hsienching.github.io/>) has no title attribute.

## 5.2 Reference-style links

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

# 6. Figures

## 6.1 Bitmap figures

Supported file format: PNG, JPG, WebP (tested.)

### 6.1.1 Normal input by Markdown

The resolution of the original figure is 2,400 × 1,600 pixels.  
(Source file: <https://commons.wikimedia.org/wiki/File:Classical_Guitar_labelled_english.jpg>)

The figure fit the column width.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Classical_Guitar_labelled_english.jpg/1024px-Classical_Guitar_labelled_english.jpg)

### 6.1.2 Change figure size by Markdown

Change figure size to 50%.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Classical_Guitar_labelled_english.jpg/1024px-Classical_Guitar_labelled_english.jpg){:height="50%" width="50%"}

## 6.2 Vector figures

Supported file format: SVG (tested.)

### 6.2.1 Normal input by Markdown

Jekyll support SVG file (vector figure) rendering.  
(Source file: <https://commons.wikimedia.org/wiki/File:Earth_poster.svg>)

The figure fit the column width.

![](https://upload.wikimedia.org/wikipedia/commons/0/07/Earth_poster.svg)

### 6.2.2 Change figure size by Markdown

Change figure size to 50%.

![](https://upload.wikimedia.org/wikipedia/commons/0/07/Earth_poster.svg){:height="50%" width="50%"}

# 7. Tables

## 7.1 Basic table format

The basic table is left alignment.

|  Column name  |  Column name  |  Column name  |
|  -----------  |  -----------  |  -----------  |
|  Data         |  Data         |  Data         |
|  Data         |  Data         |  Data         |

## 7.2 Alignment

The alignment can be adjusted by `:`.

|  Column name  |  Column name  |  Column name  |
|  :----------  |  ----------:  |  :---------:  |
|  Data         |  Data         |  Data         |
|  Data         |  Data         |  Data         |

# 8. Separation lines

The separation line can be built by three `*`, `-`, or `_`. Spaces can be inserted into the symbols.

***

* * *

*****

---

- - -

-----

___

_ _ _

_____

This format cannot built the separation line.

*-_  

# 9. Footnotes

The footnote function is very similar to reference function in LaTeX. It can be treated as reference function in blog.

Footnote demo 1 [^1].  
Footnote demo 1[^1].  
Footnote demo 1.[^1]

[^1]: This is footnote 1.

Footnote demo 2.[^footnote_name]  
Footnote demo 3.[^FOOT_label]

[^footnote_name]: This is footnote 2.
[^FOOT_label]: This is footnote 3.

Please note that the footnotes will be put at the end of the post.

Comment: The footnote function is very similar to the reference function in LaTeX. It can be treated as a reference function in the blog.

# 10. Hiding content with comments

To hide content from the rendered Markdown by placing the content in an HTML comment.

```markdown
<!-- This content will not appear in the rendered Markdown -->
```

<!-- This content will not appear in the rendered Markdown -->

# 11. Backslash escapes

Markdown provides backslash escapes `\` for the following characters:

```markdown
\   backslash
`   backtick
*   asterisk
_   underscore
{}  curly braces
[]  square brackets
()  parentheses
#   hash mark
+   plus sign
-   minus sign (hyphen)
.   dot
!   exclamation mark
```

# 12. Alerts (GitHub pages feature)

Alerts are a Markdown extension based on the blockquote syntax that you can use to emphasize critical information. On GitHub, they are displayed with distinctive colors and icons to indicate the significance of the content.

There are 5 level alerts.
Syntax:  

> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.

Here are the example figure for the rendered alerts:  
![](https://docs.github.com/assets/cb-50447/mw-1440/images/help/writing/alerts-rendered.webp)

REF: <https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax>

# 13. $LaTeX$ syntax

To display equations written by $LaTeX$ syntax, `MathJax` is required.  

`MathJax` is an open-source JavaScript display engine for $LaTeX$, MathML, and AsciiMath notation that works in all modern browsers. See <http://www.mathjax.org/> for additional details about MathJax, and <https://docs.mathjax.org> for the MathJax documentation.

MathJax (Source Repository)  
<https://github.com/mathjax/MathJax-src>

Suggested Jekyll plugin of `MathJax`:  
jekyll-spaceship: <https://github.com/jeffreytse/jekyll-spaceship>  
NOTE: This plugin can be used in local site. However, for GitHub pages, a `build warning` appears. Warning message: "The github-pages gem can't satisfy your Gemfile's dependencies."

## 13.1 Inline-style equations

This sentence uses `$` delimiters to show $LaTeX$ math inline: $\sqrt{3x-1}+(1+x)^2$

This is an equation,
${f(x)=a_nx^n+a_{n-1}x^{n-1}+a_{n-2}x^{n-2}}+\cdots$,
where there is no label (tag).

## 13.2 Display-style equations

This is an equation with label (tag).
$$
{f(x)=a_nx^n+a_{n-1}x^{n-1}+a_{n-2}x^{n-2}}+\cdots \tag{1.1}
$$

This is an equation with label (tag).

\begin{equation}
(a+b) \times c = a\times c + b \times c \\
\end{equation}

\begin{equation}
(a+b) \times c = a\times c + b \times c \\
\tag{1.1}
\end{equation}

## 13.3 Issue discussion

LaTeX math expressions can be rendered natively in Markdown on GitHub.  
REF: <https://github.blog/2022-05-19-math-support-in-markdown/>

However, in other platform (such as [Minimal Mistakes Jekyll theme](https://github.com/mmistakes/minimal-mistakes)), we still require `MathJax`.

# About Markdown

> Markdown is a lightweight markup language for creating formatted text using a plain-text editor. John Gruber and Aaron Swartz created Markdown in 2004 as a markup language that is intended to be easy to read in its source code form. Markdown is widely used for blogging and instant messaging, and also used elsewhere in online forums, collaborative software, documentation pages, and readme files.
> 
> Wikipedia: Markdown  
> <https://en.wikipedia.org/wiki/Markdown>

Markdown logo:  
![](https://upload.wikimedia.org/wikipedia/commons/4/48/Markdown-mark.svg){:height="30%" width="30%"}

# Related links

John Gruber - Markdown syntax  
The Daring Fireball Company LLC.  
<https://daringfireball.net/projects/markdown/syntax>

# Appendix: Markdown template file

Template link: <https://github.com/HsienChing/HsienChing.github.io/blob/master/_posts/2024-03-31-GitHub-pages-post-template-01.md>
