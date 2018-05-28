---
title: javacript 原型链及继承
date: 2016-08-25 18:50:30
categories: "js高级"
---



图文并茂，生动的讲解js原型链及继承。

## 6.1 定义
对象继承属性的一个链条
#  6.2构造函数,实例与原型对象的关系

<!-- more -->
```
var Person = function (name) { this.name = name; }//person是构造函数
var o3personTwo = new Person('personTwo')//personTwo是实例
```
![](/images/1.png)
![](/images/2.png)

原型对象都有一个默认的constructor属性指向构造函数
### 6.3 创建实例的方法
1.字面量
```
let obj={'name':'张三'}
2.Object构造函数创建
let Obj=new Object()
	Obj.name='张三'
3.使用工厂模式创建对象
function createPerson(name){
 var o = new Object();
 o.name = name;
 };
 return o; 
}
var person1 = createPerson('张三');
4.使用构造函数创建对象
function Person(name){
 this.name = name;
}
var person1 = new Person('张三');
```
## 6.4 new运算符
1.创了一个新对象;
2.this指向构造函数;
3.构造函数有返回,会替换new出来的对象,如果没有就是new出来的对象
4.手动封装一个new运算符
```
var new2 = function (func) {
    var o = Object.create(func.prototype); //创建对象
    var k = func.call(o);　　　　　　　　　　//改变this指向，把结果付给k
    if (typeof k === 'object') {　　　　　　//判断k的类型是不是对象
        return k;　　　　　　　　　　　　　　//是，返回k
    } else {     
        return o;　　　　　　　　　　　　　　　//不是返回返回构造函数的执行结果
    }
}  
```
## 6.5 对象的原型链

![](/images/3.png)

## 7.继承的方式
JS是一门弱类型动态语言,封装和继承是他的两大特性
### 7.1原型链继承
将父类的实例作为子类的原型
1.代码实现
```
定义父类:
// 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){  
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
子类:
function Cat(){ 
}
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';

//　Test Code
var cat = new Cat();
console.log(cat.name);//cat
console.log(cat.eat('fish'));//cat正在吃：fish  undefined
console.log(cat.sleep());//cat正在睡觉！ undefined
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true
```
2.优缺点
简单易于实现,但是要想为子类新增属性和方法，必须要在new Animal()这样的语句之后执行,无法实现多继承
### 7.2 构造继承
实质是利用call来改变Cat中的this指向
1.代码实现
子类:
```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
```
2.优缺点
可以实现多继承,不能继承原型属性/方法
### 7.3 实例继承
为父类实例添加新特性，作为子类实例返回
1.代码实现
子类
```
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}
```
2.优缺点
不限制调用方式,但不能实现多继承
### 7.4 拷贝继承
将父类的属性和方法拷贝一份到子类中
1.子类:
```
function Cat(name){
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}
```
2.优缺点
支持多继承,但是效率低占用内存
### 7.5 组合继承
通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
1.子类:
```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
```
### 7.6 寄生组合继承
```
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
```
### 7.7 ES6的extends继承
ES6 的继承机制是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this,
```
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}  
```