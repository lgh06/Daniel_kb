---
  tags:
    - 知识管理
---


jekyll博客太简陋了，markdown一篇一篇写得难受。  
准备转战Obsidian或者Joplin了，然后找一找导出静态HTML的方法。
用百度网盘的同步盘+Git来双重备份。

以下是一些能把Obsidian导出成HTML的项目：  

https://github.com/obsidian-html/obsidian-html  
https://github.com/jobindj/obsidian-publish-mkdocs  
https://github.com/UlisseMini/oth


Docsify不用build HTML，直接前端JS 解析md，转换成可阅读的格式。SEO不友好。可以直接在Github Pages上面玩玩。
Hexo看起来也不错，可以上传到Cloudflare pages玩玩。

目前此文件夹下的index.html和README.md是Docsify要求的；
config.yaml是obsidian-html这个项目要求的。
obsidian_html.bat是用obsidian-html来build html的，输出在output目录。

## Mod
Cloudflare Pages只支持3.7版本的Python，然而 https://github.com/obsidian-html/obsidian-html  用了3.9版本Python的一些特性，所以fork并修改了以下，代码在这里：
https://github.com/lgh06/obsidian-html