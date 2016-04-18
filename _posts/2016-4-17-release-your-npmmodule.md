---
layout:  post
title:  发布自己的npm包
tags: npm
description: 如何发布自己的npm包，注册，init，adduser，whoami，publish ...
---

### 如何发发布自己的npm包

### 前提
 
      
### 开始
* 安装node和npm
  去 [http://nodejs.org](http://nodejs.org)下载nodejs，同时就安装了npm
* 检查npm是否可以用
  {% highlight ruby lineanchors %}
  $npm
  {% endhighlight %}
* 创建自己的代码工程并进行初始化信息
  {% highlight ruby lineanchors %}
  $ mkdir xxxx && cd xxxx
  $ npm init
  {% endhighlight %}

* 项目结构

 {% highlight ruby lineanchors %}
xxxx
├─┬ lib
│ └── xxxx.js
├─┬ test
│ └── all.js
├── .gitignore
├── .npmignore
├── .travis.yml
├── index.js
├── LICENSE
├── makefile
├── package.json
├── README.md
 {% endhighlight %}

* 注册账户
 前往[https://www.npmjs.com](https://www.npmjs.com)

* 发布
{% highlight ruby lineanchors %}
 $ npm adduser
{% endhighlight %}

{% highlight ruby lineanchors %}
 $ npm whoami 
 $ npm publish --tag 0.1.0
{% endhighlight %}
