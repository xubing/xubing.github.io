---
layout:  post
title:  Javascript
tags: ES6 Javascript JS 
description: ES6-JS的简单语法回顾
comments: true
---

### JS
es

### 对象  ｛｝

{% highlight javascript  linenos%}

{% endhighlight javascript %}

#### 设置

```javascript
eslint --init
```

会有很多提示，比如JSX支持，ES6等，你可以自己进行step by step 生成配置文件。
我的配置

{% highlight json %}
{
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": "eslint:recommended",
    "installedESLint": true,
    "parserOptions": {
        "ecmaFeatures": {
            "experimentalObjectRestSpread": true,
            "jsx": true
        },
        "sourceType": "module"
    },
    "plugins": [
        "react"
    ],
    "rules": {
        "indent": [
            "error",
            "tab"
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "double"
        ],
        "semi": [
            "error",
            "never"
        ]
    }
}
{% endhighlight json %}

### 用法
```javascript
eslint a.js b.js
```
