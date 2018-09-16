# 面向对象 && 原型系统prototype

## 顶层函数对象 Object、Function
JS中:”一切皆是对象, 函数也是” 但对象也是有区别的。
+ 普通对象
+ 函数对象

**凡是通过 new Function() 创建的对象都是函数对象其他的都是普通对象**

+ 所有的对象都具备[[proto]]属性，指向显示创建它 函数对象的*原型对象*，否则指向Object.prototype；
+ 函数对象具备 prototype属性指向它的原型.
+ 原型对象具备 constuctor属性指向*函数对象*
```javascript
Function.__proto__ === Function.prototype

Function.prototype.constuctor === Function

Object.__proto__ === Function.prototype

Function.prototype.__proto__ === Object.prototype

Object.prototype.__proto__ === null
```
属性分为两类：
+ 数据属性 [[Value]] [[Writable]]
+ 访问器属性 [[Get]] [[Set]]

>Object.defineProperty(obj,property,{value:,enumerable:,configurable:,writable:})

通用字段[[Enumerable]]决定是否可遍历该对象
[[configurable]]决定该属性是否可配置 delete删除

```javascript
    var person1 = {
        _name:'XY',
        get name(){}
        set name(value){}
    }

    Object.preventExtensions()
    Object.isExtensible()
    Object.seal()
    Object.freeze(person1);
```

## 继承写法

```javascript
 function inherit(Base,Sub){
     var F = function(){};
     F.prototype = Base.prototype;
     Sub.prototype = new F;
     Sub.prototype.constuctor = Sub;
     
     /**
     属性是代表[对象]的状态，属于对象私有
     上面的代码 使用一个 空函数 F作为中转，
     而不是
     Sub.prototype = new Base
     目前是避免耗费空间，将Base的成员属性在保存一份
     */
 }
 //等效的写法
 Sub.prototype = Object.create(Base.prototype);
//在子类中 调用父的 构造器函数
 function Sub (){
     Base.call(this);
     //子类[成员属性]的初始化
 }
```

## 基于原型产生对象
+ RTTI 

>a intanceof A 工作原理 检查A.prototype 是否在a.__proto__原型链表上

+ let a = new A,new操作符用于**构建原型链**
> javascript 创建对象的方式是基于**原型**,以现有对象为基础创建新对象，并修改原型链
```javascript
    a = {}; //a = Object.create(Object.prototype);
    a.__proto__ = A.prototype
    A.call(a)
```
+ let a = {}
```javascript
    let a = Object.create(Object.prototype);
```

+ function A(){}
```javascript
    A.prototype = Object.create(Object.prototype,{constuctor:{
        configurable:true,
        enumerable:true,
        value:A,
        writable:true
    }})
```

## 其它
+ in 操作符 检查对象上是否存在某个属性，自有属性和原型属性
+ hasOwnProperty() 检查自有属性
+ Object.keys() 返回自身可枚举的属性