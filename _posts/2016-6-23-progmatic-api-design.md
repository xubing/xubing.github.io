---
layout:  post
title: 设计可编程的RESTfull的API的最好实践
tags: API Design
description: 设计可编程的RESTfull的API的最好实践
comments: true
---

### 前言
由于API一旦发布，很难在进行改编。但是在api设计的时候，听从几点建议，将会使你的api适应性特别强
。
这篇文章主要参考了

 - [这里](http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#restful)。
 - [还有这个](http://dev.enchant.com/api/v1)
 
 下面是总结的几个要点

#### API的主要要求 
- 对于现在的一些模糊标准，感觉不好，不要尝试去满足它。
- 应该对开发者友好， 应该可以通过浏览器进行探索访问
- 应该是简洁，一致的，很容易适配拓展

#### Use RESTful URLs and actions
  - RESTful的一个概念就是获得很好的适应性。一个关键概念就是把你的API都能分割成逻辑资源，这些资源通过http的方法进行操作。什么可以变成资源呢，从开发者角度，资源应该是名词而不是动词。虽然你可以把内部的model映射成资源，但是没有必要一一对应，不要给你的API去暴露无关的细节。比如 ticket, user and group
  
  - 一旦你的资源定义了，你需要定义动作来对它们操作和如何影射到你的API上。RESTful的原则是提供处理CRUD的action的策略。例如：

{% highlight java %}
GET /tickets - Retrieves a list of tickets
GET /tickets/12 - Retrieves a specific ticket
POST /tickets - Creates a new ticket
PUT /tickets/12 - Updates ticket #12
PATCH /tickets/12 - Partially updates ticket #12
DELETE /tickets/12 - Deletes ticket #12
{% endhighlight java %}

- 资源名称应该复数。比如  /tickets /tickets/12 
- 如何处理资源关系。如果关系仅仅是存在另外一个资源中，可以这样。

{% highlight java %}
GET /tickets/12/messages - Retrieves list of messages for ticket #12
GET /tickets/12/messages/5 - Retrieves message #5 for ticket #12
POST /tickets/12/messages - Creates a new message in ticket #12
PUT /tickets/12/messages/5 - Updates message #5 for ticket #12
PATCH /tickets/12/messages/5 - Partially updates message #5 for ticket #12
DELETE /tickets/12/messages/5 - Deletes message #5 for ticket #12
{% endhighlight java %}

- 如果actions不能满足当前CRUD操作，有几条建议
	* 重构action，使它能够像一个资源的field。
	* 把它看成一个子资源。例如PUT 

{% highlight java %}
	PUT /gists/:id/star  
	DELETE /gists/:id/star
{% endhighlight java %}
	* 有时实在没法进行影射，可以重新设置一个特殊的资源。
-  SSL 任何地方，任何时候都需要。Always use SSL. No exceptions.
-  API文档的文档应该清晰。
-  版本控制。其实关于放在URL还是header中呢，URL中比较好。
-  资源的过滤 排列和搜索
	- Filtering 过滤。对每个fieldyo对应唯一的查询参数。例如 GET /tickets?state=open
	- Sorting. 类似Fliter，sort用来描述排列规则。复杂的排列规则是一个逗号分隔的列表。负号表示下降排列。
 
 {% highlight java %}
 	GET /tickets?sort=-priority - Retrieves a list of tickets in descending order of priority
 	GET /tickets?sort=-priority,created_at - Retrieves a list of tickets in descending order of priority. Within a specific priority, older tickets are ordered first 	
{% endhighlight java %}
 
 - Searching 搜索。如果几本的过滤不能满足的时候，需要进行全文搜索了。
 - 通过API限制啊返回的field。使用带逗号的fields来查询。
 ```
 GET /tickets?fields=id,subject,customer_name,updated_at&state=open&sort=-updated_at
 ```
 - JSON only responses
 - ensure gzip is supported
 - 数据不要使用默认分装，除非需要。
 - JSON encoded POST, PUT & PATCH bodies
 - Pagination

	
	
	
	 