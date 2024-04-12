---
title:  "GitHub pages post markdown file template 01"
date: 2024-03-31 23:30:00 +0800
excerpt: "A markdown template file for GitHub pages"
categories: 
  - GitHub pages
tags:
  - GitHub pages template
toc: true
toc_sticky: false
toc_label: "Table of contents"
toc_icon: "columns"
---

About this template file:

This is a markdown template file for GitHub pages.

A suggested way to use this file is to see the source file, and then copy-paste the function you need. Try the code and modify the code in your style.

Template link: <https://github.com/HsienChing/HsienChing.github.io/blob/master/_posts/2024-03-31-GitHub-pages-post-template-01.md>

# 1. Section heading

Normal text.

## 1.1 Subsection heading

Normal text.

### 1.1.1 Subsubsection heading

Normal text.

#### 1.1.1.1 4-th level heading

Normal text.

##### 1.1.1.1.1 5-th level heading

Normal text.

###### 1.1.1.1.1.1 6-th level heading

Normal text.

# 2. Text style

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

# 3. List

## 3.1 Bullet list

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

## 3.2 Numbered list

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

# 5. Hyperlink

## 5.1 Inline-style link

### 5.1.1 

Use `<` and `>`

Link: https://hsienching.github.io/

Link: <https://hsienching.github.io/>

### 5.1.2

This is [My GitHub pages](<https://hsienching.github.io/> "Title") inline link.

[This link](<https://hsienching.github.io/>) has no title attribute.

## 5.2 Reference-style link

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

# 6. Figure

## 6.1 Bitmap figure

### 6.1.1 Normal input by markdown

The resolution of the original figure is 2,400 × 1,600 pixels.  
(Source file: <https://commons.wikimedia.org/wiki/File:Classical_Guitar_labelled_english.jpg>)

The figure fit the column width.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Classical_Guitar_labelled_english.jpg/1024px-Classical_Guitar_labelled_english.jpg)

### 6.1.2 Change figure size by markdown

Change figure size to 50%.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Classical_Guitar_labelled_english.jpg/1024px-Classical_Guitar_labelled_english.jpg){:height="50%" width="50%"}

## 6.2 Vector figure

### 6.2.1 Normal input by markdown

Jekyll support SVG file (vector figure) rendering.  
(Source file: <https://commons.wikimedia.org/wiki/File:Earth_poster.svg>)

The figure fit the column width.

![](https://upload.wikimedia.org/wikipedia/commons/0/07/Earth_poster.svg)

### 6.2.2 Change figure size by markdown

Change figure size to 50%.

![](https://upload.wikimedia.org/wikipedia/commons/0/07/Earth_poster.svg){:height="50%" width="50%"}

# 7. Table 

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

# 8. Separation line

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

# 9. Footnote

Footnote demo 1 [^1].  
Footnote demo 1[^1].  
Footnote demo 1.[^1]

[^1]: This is footnote 1.

Footnote demo 2.[^footnote_name]  
Footnote demo 3.[^FOOT_label]

[^footnote_name]: This is footnote 2.
[^FOOT_label]: This is footnote 3.

Please note that the footnotes will be put at the end of the post.

# 10. LaTeX syntax

To display equations written by LaTeX syntax, `MathJax` is required.

Suggested Jekyll plugin of `MathJax`:  
jekyll-spaceship: <https://github.com/jeffreytse/jekyll-spaceship>

## 10.1 Inline-style equation

This is an equation.
${f(x)=a_nx^n+a_{n-1}x^{n-1}+a_{n-2}x^{n-2}}+\cdots$
This is an equation.

## 10.2 Display-style equation

This is an equation.
$$
{f(x)=a_nx^n+a_{n-1}x^{n-1}+a_{n-2}x^{n-2}}+\cdots \tag{1.1}
$$
This is an equation.

\begin{equation}
(a+b) \times c = a\times c + b \times c \\
\end{equation}

\begin{equation}
(a+b) \times c = a\times c + b \times c \\
\tag{1.1}
\end{equation}




# Appendix: Markdown template file

Template link: <https://github.com/HsienChing/HsienChing.github.io/blob/master/_posts/2024-03-31-GitHub-pages-post-template-01.md>
