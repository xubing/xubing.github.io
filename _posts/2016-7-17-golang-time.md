---
layout:  post
title:  Golang基础-Time
tags: Golang 
description:  Golang基础-Time
comments: true
---

### 格式化时间
2006-01-02T15:04:05Z07:00的意义是
1 2 3 4 5 6 7
月 日 时分秒 年 时区。这个还是有一定记忆规则的。

{% highlight golang linenos %}
t := time.Unix(1362984425, 0)
nt := t.Format("2006-01-02 15:04:05")
fmt.Println(nt)
{% endhighlight golang %}