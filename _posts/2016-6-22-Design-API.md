---
layout:  post
title:  API设计规则&指导
tags: API Design
description: API的设计规则
comments: true
---
### 前沿
   Good API design is hard! 几篇传送门文章。

   - [API设计](http://www.cnblogs.com/moonz-wu/p/4211626.html)
   - [设计指南](http://www.360doc.com/content/15/0206/09/15077656_446615750.shtml)
   
 
     
### 设计原则
-  只做满足当前需求的，经受住明天问题的诱惑
-  完备
-  不冗余 提供最小的功能集合。
-  清晰简单
-  易于学习 接口设计有统一的范式
-  可扩展
-  API版本控制.可以把版本号放在请求url的根目录下。
-  
### 本地API设计
API设计的比较好，对将来的扩展或者理解都有很大帮助。

- 一致性。有open就需要有close对应。
- 对称性。
	```
	 add(object) 
	 addAll(objects)
	 remove(object)
	 removeAll()
	 ```
- 返回值。保持一处返回，返回结果类型一致。

### HTTP API 设计
- 简单 隔离。 通过请求的不同部分来隔离，使事情简单化。
- 指定版本号
- 支持Etag缓存
- 为每一个请求包含一个Request－id字段。可以跟踪 诊断和调试请求。
- 返回合适的状态码。
- 请求body使用json数据。
- 资源名／行为。 如。runner/{id}/actions/stop

	
	  
