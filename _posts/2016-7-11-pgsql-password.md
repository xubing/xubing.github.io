---
layout:  post
title:  pgsql密码配置
tags: pgsql
description: pgsql
comments: true
---

### 常用传送门

- [原文](http://davidx.me/2015/09/01/run-psql-without-password-prompt/)
	
	运行psql的时候，如果没有密码，可以创建一个666权限的 **$HOME/.pgpass**文件。
	文件格式

``` 
	# host:port:database:username:password
	localhost:5432:*:postgres:postgres
```
	