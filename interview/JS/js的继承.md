# 前言

许多的语言的继承支持两种方式：接口继承和实现继承。但是在ECMAScript只支持实现继承，而且主要依靠的是原型链来实现的。所以在理解JS继承前，要先了解原型链的概念。

# 1.原型链

## 1.1.构造函数，原型和实例之间的关系
每个构造函数（`Person`）都含有一个原型对象（`Person.prototype`），而原型对象都包含一个指向构造函数的指针（`constructor`），每个实例（`Jack`）都包含一个指向原型对象的指针（`[[prototype]]`）。
![enter image description here](https://qiniu.cqz21.top/%E5%8E%9F%E5%9E%8B%E9%93%BE1.JPG)

原型链就是在上面的基础上，把`Person Prototype`中原本指向构造函数（`Person`）的指针，指向另一个新的构造函数（`Animal`）的原型（`Animal Prototype`）上，以此下去就形成所谓的原型链。
![enter image description here](https://qiniu.cqz21.top/%E5%8E%9F%E5%9E%8B%E9%93%BE2.JPG)

## 1.2.默认的原型
事实上所有的引用类型都是默认继承了Object，而这也是通过原型链实现的继承，所以在1.1中的例子需要再加上默认的这一环。
![enter image description here](https://qiniu.cqz21.top/%E5%8E%9F%E5%9E%8B%E9%93%BE3.JPG)

## 1.3.确定原型和实例的关系

**1.3.1. 使用instanceof操作符：**
```javascript
alert(Jack instanceof Object);//true
alert(Jack instanceof Animal);//true
alert(Jack instanceof Person);//true
```
由上面的语句可以看出，实例和原型链中出现的实例返回的都是true。也可以说`Jack`是原型链中任何一个类型的实例。

**1.3.2.使用isPrototypeOf()方法：**
```javascript
alert(Object.prototype.isPrototypeOf(Jack));//true
alert(Person.prototype.isPrototypeOf(Jack));//true
alert(Animal.prototype.isPrototypeOf(Jack));//true
```

## 1.4.谨慎地定义方法

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQwNjE0MzUwN119
-->