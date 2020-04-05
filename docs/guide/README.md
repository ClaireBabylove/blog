---
sidebar: auto
---

# call和apply原理解析

## call模拟实现

首先请看示例
``` js
var value = 1
var foo = {
    value: 2
}
function bar() {
	console.log(this.value)
}
bar.call(foo) // 2
```
通过上面代码，我们知道call()主要有一下两点:  
1、call()改变了this指向
2、执行了bar函数
那么可以为foo添加属性bar，调用函数bar,然后再将其删除
``` js
Function.prototype.call2 = function(context){
    context = context ? Object(context): window;
    context.fn = this;
    let args = [...arguments].slice(1)
    let result = context.fn(...args)
    
    delete context.fn
    return result;
}
```
``` js
// 验证：
var foo = {
    value: 1
}
   
function bar(name, age) {
    console.log(this.value, name, age)
}

bar.call2(foo, 'Claire', 18) // 1 "Claire", 18
```
注:
1、Object构造函数为给定值创建一个对象包装器，如果给定值是null或undefined，将会创建并返回一个空对象，否则将返回一个与给定值对应类型的对象。当以非构造函数形式被调用时，Object等同于new Object()。
2、slice()方法从已有的数组中返回选定的元素。返回值为一个新数组。
3、this指向，谁调用指向谁。
## apply模拟实现
``` js
Function.prototype.apply2 = function(context, arr) {
    context = context ? Object(context) : window;
    context.fn = this;
    let result;
    if(!arr) {
        result = context.fn()
    } else {
        result = context.fn(...arr)
    }
    delete context.fn
    return result;
}
```

