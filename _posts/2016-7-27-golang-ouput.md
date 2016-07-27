---
layout:  post
title:  Golang基础-输出
tags: Golang 
description:  Golang基础-Output
comments: true
---

### 定义输出的结构

有的时候内部定义的db struct并不是要输出的结果形式。那么可以快速定义输出的结构。如下：

{% highlight golang linenos %}
package main

import (
	"encoding/json"
	"os"
)


type Model struct {
	DeletedAt int `json:"deletedAt"`
}

// Defines the database model
type User struct {
	*Model
	Username  string `json:"username"`
	Password string`json:"password"`
}

// Final JSON output
type PublicUser struct {
	*User
	DeletedAt bool `json:"deletedAt,omitempty"`
	Password  bool `json:"password,omitempty"`
}


func main()  {

	var p = PublicUser{ &User{ &Model{2000},"bob","12345pwd"},false,false}
	ret,err := json.Marshal(p)
	if err != nil {
		fmt.Println("parse error")
	}
	fmt.Println(string(ret)) //{"username":"bob"}

}

{% endhighlight golang %}