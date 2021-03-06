---
layout:  post
title:  ES6之Promise
tags: ES6 Promise
description: Promise
comments: true
---

### [定义](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
The Promise object is used for deferred and asynchronous computations. A Promise represents an operation that hasn't completed yet, but is expected in the future.
### 语法

```javascript
new Promise( /* executor */ function(resolve, reject) { ... } );
```
#### 参数
executor
A function that will be passed to other functions via the arguments resolve and reject. The executor function is executed immediately by the Promise implementation which provides the resolve and reject functions (the executor is called before the Promise constructor even returns the created object). The resolve and reject functions are bound to the promise and calling them fulfills or rejects the promise, respectively. The executor is expected to initiate some asynchronous work and then, once that completes, call either the resolve or reject function to resolve the promise's final value or else reject it if an error occurred.

### 一个Promise只有三种状态 
* pending: initial state, not fulfilled or rejected.
* fulfilled: meaning that the operation completed successfully.
* rejected: meaning that the operation failed.
![图示](https://mdn.mozillademos.org/files/8633/promises.png){: .center-image }

### 例子
``` javascript
function hello(ready){
  return new Promise((resolv,reject)=>{
    if(ready)
    {
      resolv("Hello");
    }
    else{
      var date = new Date();
      console.log(date)
      reject({message:"Bye",date:date});
    }
  });
}

hello(false).then((message)=>
{
    console.log(message);
},
(obj)=>
{
    console.log(obj);
}
);

```

### 例子2 利用then进行顺序执行。由于每次方法的执行总是返回一个Promise对象。
``` javascript
function printHello (ready) {
    return new Promise(function (resolve, reject) {
        if (ready) {
            resolve("Hello");
        } else {
            reject("Good bye!");
        }
    });
}

function printWorld () {
    console.log("World");
}

function printExclamation () {
    console.log("!");
}

printHello(false)
    .then(function(message){
        console.log(message);
    },
    function(message){
        console.log('error:',message);
    }
  )
    .then(printWorld)
    .then(printExclamation);    
```

### catch. catch 方法是 then(onFulfilled, onRejected) 方法当中 onRejected 函数的一个简单的写法，也就是说可以写成 then(fn).catch(fn)，相当于 then(fn).then(null, fn)。使用 catch 的写法比一般的写法更加清晰明确。
