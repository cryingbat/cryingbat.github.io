---
title: javacript 常见五种设计模式
date: 2018-05-26 19:42:39
categories: "js高级"
---

- 非设计模式
    “简单”就是一把钥匙开一把锁的模式，目的仅仅是着眼于解决现在的问题，而设计模式的“复杂”就在于它是要构造一个“万能钥匙”，目的是提出一种对所有锁的开锁方案。

- 设计模式
    要使代码可被反复使用,请用'设计模式'对你的代码进行设计.
    <!-- more -->
    很多我所认识的程序员在接触到设计模式之后，都有一种相见恨晚的感觉，有人形容学习了设计模式之后感觉自己好像已经脱胎换骨，达到了新的境界，还有人甚至把是否了解设计模式作为程序员划分水平的标准。
    我们也不能陷入模式的陷阱，为了使用模式而去套模式，那样会陷入形式主义。我们在使用模式的时候，一定要注意模式的意图（intent），而不 要过多的去关注模式的实现细节，因为这些实现细节在特定情况下，可能会发生一些改变。不要顽固地认为设计模式一书中的类图或实现代码就代表了模式本身。
    
## 5.1 工厂模式
简单的工厂模式可以理解为解决多个相似的问题。
```
function CreatePerson(name,age,sex) {
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.sex = sex;
    obj.sayName = function(){    
        return this.name;
    }    
    return obj;
}
var p1 = new CreatePerson("longen",'28','男');
var p2 = new CreatePerson("tugenhua",'27','女');
console.log(p1.name); // longen
console.log(p1.age);  // 28
console.log(p1.sex);  // 男
console.log(p1.sayName()); // longen

console.log(p2.name);  // tugenhua
console.log(p2.age);   // 27
console.log(p2.sex);   // 女
console.log(p2.sayName()); // tugenhua  
```
## 5.2单例模式
只能被实例化(构造函数给实例添加属性与方法)一次
// 单体模式
```
var Singleton = function(name){
    this.name = name;
};
Singleton.prototype.getName = function(){
    return this.name;
}
// 获取实例对象
var getInstance = (function() {
    var instance = null;    
    return function(name) {    
        if(!instance) {//相当于一个一次性阀门,只能实例化一次
            instance = new Singleton(name);
        }        
        return instance;
    }
})();
// 测试单体模式的实例,所以a===b
var a = getInstance("aa");
var b = getInstance("bb");
```
## 5.3 沙箱模式
将一些函数放到自执行函数里面,但要用闭包暴露接口,用变量接收暴露的接口,再调用里面的值,否则无法使用里面的值。
```
let sandboxModel=(function(){    
    function sayName(){};    
    function sayAge(){};    
    return{    
        sayName:sayName,
        sayAge:sayAge
    }
})()
```
## 5.4 发布者订阅模式
就例如如我们关注了某一个公众号,然后他对应的有新的消息就会给你推送
//发布者与订阅模式
  ```
    var shoeObj = {}; // 定义发布者
    shoeObj.list = []; // 缓存列表 存放订阅者回调函数

    // 增加订阅者
    shoeObj.listen = function(fn) {
        shoeObj.list.push(fn); // 订阅消息添加到缓存列表
    }

    // 发布消息
    shoeObj.trigger = function() {
           for (var i = 0, fn; fn = this.list[i++];) {
               fn.apply(this, arguments);//第一个参数只是改变fn的this,
            }
        }
     // 小红订阅如下消息
    shoeObj.listen(function(color, size) {    
         console.log("颜色是：" + color);        
         console.log("尺码是：" + size);
    });
         
    // 小花订阅如下消息
    shoeObj.listen(function(color, size) {     
        console.log("再次打印颜色是：" + color);        
        console.log("再次打印尺码是：" + size);
    });
    shoeObj.trigger("红色", 40);
    shoeObj.trigger("黑色", 42);  
    ```

代码实现逻辑是用数组存贮订阅者, 发布者回调函数里面通知的方式是遍历订阅者数组,并将发布者内容传入订阅者数组。


## 5.5 构造函数模式

```
/**
 * 构造一个动物的函数 
 */
function Animal(name, color){
    this.name = name;
    this.color = color;
    this.getName = function(){
        return this.name;
    }
}
// 实例一个对象
var cat = new Animal('猫', '白色');
console.log( cat.getName() );
```