---
layout:     post
title:      call、apply、bind
date:       2017-11-7
author:     zrd
catalog:    true
tags:
    - JS
---

# call、apply

### 语法

`fun.call(thisArg, arg1, arg2...)`

`fun.apply(thisArg, [argsArray])`

注：第一个参数为null/undefined时，this会指向默认的宿主对象，浏览器中就是window，严格模式下为null/undefined。apply第二个参数是数组或类数组。

### 用途

1. 改变函数内部this指向
2. 借用其他对象的方法
   例：操作`arguments`的时候，经常借用`Array.prototype`的方法。
       比如说往`arguments`中添加一个参数,使用`Array.prototype.push`
       V8引擎中实现：
       
       ```
       function ArrayPush(){
         var n = TO_UNIT32(this.length);    //被push对象的length
         var m = %_ArgumentsLength();       //push的参数个数
         for(var i = 0; i < m; i++){
           this[i + n] = %_Arguments(i);    //复制元素
         }
         this.length = n + m;               //修正length属性
         return this.length;
       }
       ```
       
       可以看出push只是属性复制的过程并修改length属性,被修改的对象是谁并不重要。因此可以借用push方法的对象要满足两个条件：
       - 对象本身可以存取属性
       - 对象的length属性可以读写
       
## bind

### 语法

fun.bind(thisArg[, arg1[, arg2[, ...]]])

注：当fun通过new来使用时,第一个参数会被忽略,但后面的参数不会忽略。

### bind实现

```

Function.prototype.bind = function(oThis) {
  //判断调用bind的对象是不是函数，若不是则报错
  if (typeof this !== 'function') {
    throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable');
  }
  
  //取得传入bind的除了第一个参数的参数,函数柯里化
  var aArgs   = Array.prototype.slice.call(arguments, 1),
      
      //保存调用bind的对象
      fToBind = this,
      
      //创建一个空对象
      fNOP = function() {},
      
      //最后返回的函数,this为调用fBound函数时的执行环境,若不是fNOP的实例，则将oThis绑定到调用bind的函数中的this上,然后将传入fBound的参数与aArgs
        合并传入到调用bind的函数中。
      //当fBound被当做构造函数来使用时,this是fBound的实例，而fBound的原型是一个fNOP对象,所以this也是fNOP的实例,此时将忽略oThis,后续参数会继续传入。
      fBound  = function() {
        return fToBind.apply(this instanceof fNOP ? this : oThis, aArgs.concat(Array.prototype.slice.call(arguments)));
      };
  
  //若没有下面两句的话,fBound被用作构造函数调用时,创建的实例将不会继承调用bind对象的属性
  if (this.prototype) {
     fNOP.prototype = this.prototype; 
  }
  
  //这里为什么不直接使用fBound.prototype = this.prototype？因为这样修改fBound.prototype会改变this.prototype,所以使用一个空对象中转。
  fBound.prototype = new fNOP();

  return fBound;
}

```























       
       
       
       
       
       
       
       
