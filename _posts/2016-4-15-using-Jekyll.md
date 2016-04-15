---
layout: post
title: 在github上使用jekyll做自己的github
---
## 写在前面的话
   以前写过不少博客。经常没有坚持下去。地方很分散，很早的csdn，博客园，网易都有。后来在SAE上自己用word press写自己的博客。写了2年后，发现积累了很多，前几天登录发现我的应用居然删除了，所有东西都没了。因为SAE中我以前有别的app，导致云豆欠费后，所有东西清空，欲哭无泪。不仅仅是我的app所有数据没有了，近3年的博客内容都删除了。88 SAE。不骂你，但是我以后不会再碰你了。平时Evernote中也写了几百篇，害怕以后evernote也夸掉，还是用md进行写博客吧。
	本来想在阿里云或者AWS上弄一个服务器，想想github page还是很好用的，所以就有了这个页面。github page就是username.github.io，这个自己可以创建的。可以查看github中的说明。
	
	
### Jekyll的安装使用
1. 参见https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/的文章。
2. 不使用模版的步骤。jekyll new  ，jekyll build，jekyll server ,本地就可以查看了。然后git 提交自己的所有文件。
3. Jekyll的模版。先fork 模版 https://github.com/barryclark/jekyll-now
4. 然后在自己的工程中点开setting，然后rename成 username.github.io
5. 这个时候访问username.github.io，修改_config.yml文件，然后就可以看到页面了。
6. 以后在_post添加文件,year-month-day-title.md这样的命名，然后添加提交，github会帮你进行生成页面。自己不用进行build
7. 补充下本地运行。git clone下来自己的工程，切换到目录下，运行jekyll server。我遇到的2个问题是cannot load such file -- jekyll-sitemap。 直接gem install jekyll-sitemap。反正缺少哪个文件，就安装哪个文件。然后就可以了。
8. 有一个在线工具不错，http://prose.io 想用的也可以尝试下。
9. 我用编辑md文件的IDE是Mou，可以一边编辑一边预览。
10. 有的时候本地运行出现错误。可以用sudo gem cleanup ，bundle exec jekyll serve进行清理启动

