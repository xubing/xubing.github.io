---
layout:  post
title: 设计可编程的RESTfull的API的最好实践
tags: API Design
description: 设计可编程的RESTfull的API的最好实践
comments: true
---

### 前言
由于API一旦发布，很难在进行改编。但是在api设计的时候，听从几点建议，将会使你的api适应性特别强。免费转载请注明引用自[Bing's Blog](http://xubing.github.io)
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

```bash
GET /tickets - Retrieves a list of tickets
GET /tickets/12 - Retrieves a specific ticket
POST /tickets - Creates a new ticket
PUT /tickets/12 - Updates ticket #12
PATCH /tickets/12 - Partially updates ticket #12
DELETE /tickets/12 - Deletes ticket #12
``` 

- 资源名称应该复数。比如  /tickets /tickets/12 
- 如何处理资源关系。如果关系仅仅是存在另外一个资源中，可以这样。

```bash
GET /tickets/12/messages - Retrieves list of messages for ticket #12
GET /tickets/12/messages/5 - Retrieves message #5 for ticket #12
POST /tickets/12/messages - Creates a new message in ticket #12
PUT /tickets/12/messages/5 - Updates message #5 for ticket #12
PATCH /tickets/12/messages/5 - Partially updates message #5 for ticket #12
DELETE /tickets/12/messages/5 - Deletes message #5 for ticket #12
``` 

- 如果actions不能满足当前CRUD操作，有几条建议
	* 重构action，使它能够像一个资源的field。
	* 把它看成一个子资源。例如PUT 

``` bash
	PUT /gists/:id/star  
	DELETE /gists/:id/star
``` 
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
 
 - Searching 搜索。如果基本的过滤不能满足的时候，需要进行全文搜索了。添加key来搜索吧 

{% highlight java %}
	GET /tickets?sort=-updated_at - Retrieve recently updated tickets
	GET /tickets?state=closed&sort=-updated_at - Retrieve recently closed tickets
	GET /tickets?q=return&state=open&sort=-priority,created_at - Retrieve the highest priority open tickets mentioning the word 'return'
{% endhighlight java %}
 
 
 - 通过API限制啊返回的field。使用带逗号的fields来查询。

{% highlight java %}
 GET /tickets?fields=id,subject,customer_name,updated_at&state=open&sort=-updated_at
{% endhighlight java %}
 
 - 更新和创建需要返回资源表现(resource representation)
 	在post的结果相应中，用201 状态码和包含Location的头部来指向新资源的URL
 - JSON only responses
 - field名称用蛇形命名而不是骆驼命名法。蛇形命名方法更容易阅读。
 - 默认完美打印和gzip支持。
 	返回提供空格会让打印出来的结果容易阅读。可以提供一个查询参数比如?pretty=true来开启完美打印功能。这种数据的额外损耗是非常小的,特别当支持压缩的时候。可以看下损耗的数据。

``` 
$ curl https://api.github.com/users/veesahni > with-whitespace.txt
$ ruby -r json -e 'puts JSON JSON.parse(STDIN.read)' < with-whitespace.txt > without-whitespace.txt
$ gzip -c with-whitespace.txt > with-whitespace.txt.gz
$ gzip -c without-whitespace.txt > without-whitespace.txt.gz
``` 
 
 输出数据的大小比较：
 
``` 
without-whitespace.txt - 1252 bytes
with-whitespace.txt - 1369 bytes
without-whitespace.txt.gz - 496 bytes
with-whitespace.txt.gz - 509 bytes
``` 

 - 数据不要使用默认分装，除非需要。
 - JSON encoded POST, PUT & PATCH bodies
	如果你遵守上面的建议的话，你会把所有的输出包装成Json格式。那么API的输入呢。
	很多API都在她们请求body中使用URL编码。URL encoding确切看起来请求的body是Key value对，对于简单的查询，这些已经满足工作了。但是URL enconding还是有一些问题的，因为它没有数据类型的概念，它会强制分析整型和布尔型等。更重要的是，它没有继承结构的概念。简单的API，UR encoding是有效的，复杂的API将会坚持JSON作为输入。
	
	一个接受Json encoded的POST PUT PATCH请求的API应该需要 **Content-Type**头部去设置**application/json**，或者跑出415不支持的状态码
	
 - Pagination 分页。

``` 
Link: <https://api.github.com/user/repos?page=3&per_page=100>; rel="next", <https://api.github.com/user/repos?page=50&per_page=100>; rel="last"
``` 

- 自动加载相关资源表现（Auto loading related resource representations）
 很多情况下需要加载请求中的相关数据。对开发者来说，不用重复去请求相关的数据，使用一个参数就可以满足这种情况。比如 **embed**或者**expand**	.下面例子中将会展示相关的单独的field

``` 
GET /tickets/12?embed=customer.name,assigned_user
``` 

输入可能像这样：

{% highlight json %}
{
  "id" : 12,
  "subject" : "I have a question!",
  "summary" : "Hi, ....",
  "customer" : {
    "name" : "Bob"
  },
  assigned_user: {
   "id" : 42,
   "name" : "Jim",
  }
}
{% endhighlight json %}

- Rate Limiting 访问的频率限制
	
	At a minimum, include the following headers (using Twitter's naming conventions as headers typically don't have mid-word capitalization):

	* X-Rate-Limit-Limit - The number of allowed requests in the current period
	* X-Rate-Limit-Remaining - The number of remaining requests in the current period
	* X-Rate-Limit-Reset - The number of seconds left in the current period
- Authentication 认证相关
   
  一个RESTful API 应该无状态的。这也就意味着请求认证不能依赖于cookies或者sesion，每一个请求都应该有相关的授权证书。通常使用SSL，认证证书将会通过一个继承于HTTP Basic Auth的field的随机随机产生的access token。完全可以通过浏览器进行探索检验。如果接受到401无认证的状态码，浏览球会弹出查询证书的提示。
  OAuth 2使用Bearer tokens也依赖于SSL给他的底层传输加密。
  支持JSONP的API需要一个第三个验证，因为JSONP不能发功基本的Auth认证或者Bearer tokens，这个时候需要使用一个access_token。注意，这有一个潜在安全问题，查询的时候这个token。

- HTTP status codes

{% highlight java %}
200 OK - Response to a successful GET, PUT, PATCH or DELETE. Can also be used for a POST that doesn't result in a creation.
201 Created - Response to a POST that results in a creation. Should be combined with a Location header pointing to the location of the new resource
204 No Content - Response to a successful request that won't be returning a body (like a DELETE request)
304 Not Modified - Used when HTTP caching headers are in play
400 Bad Request - The request is malformed, such as if the body does not parse
401 Unauthorized - When no or invalid authentication details are provided. Also useful to trigger an auth popup if the API is used from a browser
403 Forbidden - When authentication succeeded but authenticated user doesn't have access to the resource
404 Not Found - When a non-existent resource is requested
405 Method Not Allowed - When an HTTP method is being requested that isn't allowed for the authenticated user
410 Gone - Indicates that the resource at this end point is no longer available. Useful as a blanket response for old API versions
415 Unsupported Media Type - If incorrect content type was provided as part of the request
422 Unprocessable Entity - Used for validation errors
429 Too Many Requests - When a request is rejected due to rate limiting
{% endhighlight java %}
