---
title: Hexo使用备忘记录
categories:
  - hexo使用
tags:
  - 标签插件
  - tag Plugins
  - hexo
date: 2014-02-02 20:11:02
---

# 标签插件（Tag Plugins）
  标签插件和 Front-matter 中的标签不同，它们是用于在文章中快速插入特定内容的插件。

## 引用块
在文章中插入引言，可包含作者、来源和标题。
### 别号： quote
```
{% blockquote [author[, source]] [link] [source_link_title] %}
content
{% endblockquote %}
```
### 样例
(1) 没有提供参数，则只输出普通的 blockquote
```
{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}
```
{% blockquote %}
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque hendrerit lacus ut purus iaculis feugiat. Sed nec tempor elit, quis aliquam neque. Curabitur sed diam eget dolor fermentum semper at eu lorem.
{% endblockquote %}


(2) 引用书上的句子(带参数)
```
{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}
```
{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}


(3) 引用 Twitter（带链接）
```
{% blockquote @DevDocs https://twitter.com/devdocs/status/356095192085962752 %}
NEW: DevDocs now comes with syntax highlighting. http://devdocs.io
{% endblockquote %}
```
{% blockquote @DevDocs https://twitter.com/devdocs/status/356095192085962752 %}
NEW: DevDocs now comes with syntax highlighting. http://devdocs.io
{% endblockquote %}


(4) 引用网络上的文章(纯文字链接)
```
{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}
```
{% blockquote Seth Godin http://sethgodin.typepad.com/seths_blog/2009/07/welcome-to-island-marketing.html Welcome to Island Marketing %}
Every interaction is both precious and an opportunity to delight.
{% endblockquote %}



## 代码块
  在文章中插入代码。
### 别名： code
```
{% codeblock [title] [lang:language] [url] [link text] %}
code snippet
{% endcodeblock %}
```
{% codeblock [title] [lang:language] [url] [link text] %}
code snippet
{% endcodeblock %}

### 样例
(1) 普通的代码块
```
{% codeblock %}
alert('Hello World!');
{% endcodeblock %}
```
{% codeblock %}
alert('Hello World!');
{% endcodeblock %}

(2) 指定语言
```
{% codeblock lang:objc %}
[rectangle setX: 10 y: 10 width: 20 height: 20];
{% endcodeblock %}
```
{% codeblock lang:objc %}
[rectangle setX: 10 y: 10 width: 20 height: 20];
{% endcodeblock %}

(3) 附加说明
```
{% codeblock Array.map %}
array.map(callback[, thisArg])
{% endcodeblock %}
```
{% codeblock Array.map %}
array.map(callback[, thisArg])
{% endcodeblock %}

(4) 附加说明和网址
```
{% codeblock _.compact http://underscorejs.org/#compact Underscore.js %}
_.compact([0, 1, false, 2, '', 3]);
=> [1, 2, 3]
{% endcodeblock %}
```
{% codeblock _.compact http://underscorejs.org/#compact Underscore.js %}
_.compact([0, 1, false, 2, '', 3]);
=> [1, 2, 3]
{% endcodeblock %}

(5) 反引号代码块
另一种形式的代码块，不同的是它使用三个反引号来包裹。
```
\``` [language] [title] [url] [link text] code snippet ```\
```
```
[language] [title] [url] [link text] code snippet
```

(6) Pull Quote
在文章中插入 Pull quote。

{% pullquote [class] %}
content
{% endpullquote %}


(7) jsFiddle
在文章中嵌入 jsFiddle。
```
{% jsfiddle shorttag [tabs] [skin] [width] [height] %}
```
{% jsfiddle shorttag [tabs] [skin] [width] [height] %}


(8) Gist
在文章中嵌入 Gist。
```
{% gist gist_id [filename] %}
```
{% gist gist_id [filename] %}


(9) iframe
在文章中插入 iframe。
```
{% iframe url [width] [height] %}
```
{% iframe url [width] [height] %}


### Image
在文章中插入指定大小的图片。
```
{% img [class names] /path/to/image [width] [height] [title text [alt text]] %}
```
{% img [icon] http://7xswux.com1.z0.glb.clouddn.com/mrscorpion.png [200] [300] [mr [alt text]] %}
正确的引用图片方式是使用下列的标签插件而不是 markdown ：
```
{% asset_img example.jpg This is an example image %}
``
{% asset_img http://7xswux.com1.z0.glb.clouddn.com/mrscorpion.png This is an example image %}
通过这种方式，图片将会同时出现在文章和主页以及归档页中。

(10) Link
在文章中插入链接，并自动给外部链接添加 target="_blank" 属性。
```
{% link text url [external] [title] %}
```
{% link text url [external] [title] %}


(11) Include Code
插入 source 文件夹内的代码文件。
```
{% include_code [title] [lang:language] path/to/file %}
```
{% include_code [title] [lang:language] path/to/file %}


(12) Youtube
在文章中插入 Youtube 视频。
```
{% youtube video_id %}
```
{% youtube video_id %}


(13) Vimeo
在文章中插入 Vimeo 视频。
```
{% youtube video_id %}
```
{% vimeo video_id %}


(14) 引用文章
引用其他文章的链接。
```
{% post_path slug %}
{% post_link slug [title] %}
```
{% post_path slug %}
{% post_link slug [title] %}


(15) 引用资源
引用文章的资源。
```
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}
```
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}


(16) Raw
如果您想在文章中插入 Swig 标签，可以尝试使用 Raw 标签，以免发生解析异常。
```
{% raw %}
content
{% endraw %}
```
{% raw %}
content
{% endraw %}
