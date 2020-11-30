---
title: tag hide
date: 2020-11-30 14:21:50
tags:
---

inline 在文本里面添加按鈕隱藏內容，只限文字


{% hideInline content,display,bg,color %}


哪個英文字母最酷？ {% hideInline 因為西裝褲(C裝酷),查看答案,#FF7242,#fff %}

門裏站着一個人? {% hideInline 閃 %}


block獨立的block隱藏內容，可以隱藏很多內容，包括圖片，代碼塊等等

{% hideBlock display,bg,color %}
content
{% endhideBlock %}

查看答案
{% hideBlock 查看答案 %}
傻子，怎麼可能有答案
{% endhideBlock %}

如果你需要展示的內容太多，可以把它隱藏在收縮框裏，需要時再把它展開。

{% hideToggle display,bg,color %}
content
{% endhideToggle %}

{% hideToggle Butterfly安裝方法 %}
在你的博客根目錄裏

git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

如果想要安裝比較新的dev分支，可以

git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

{% endhideToggle %}
