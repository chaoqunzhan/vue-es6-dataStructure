# 前言

许多的语言的继承支持两种方式：接口继承和实现继承。但是在ECMAScript只支持实现继承，而且主要依靠的是原型链来实现的。所以在理解JS继承前，要先了解原型链的概念。

# 1.原型链

## 1.1.构造函数，原型和实例之间的关系
每个构造函数（`Person`）都含有一个原型对象（`Person.prototype`），而原型对象都包含一个指向构造函数的指针（`constructor`），每个实例（`Jack`）都包含一个指向原型对象的指针（`[[prototype]]`）。
![enter image description here](https://qiniu.cqz21.top/%E5%8E%9F%E5%9E%8B%E9%93%BE1.JPG)

原型链就是在上面的基础上，让原型对象`Person Prototype`等于另一个实例（`Animal`），换句话说就是`Person Prototype`作为`Animal`的实例(`Person.prototype = new Animal()`)，以此下去就形成所谓的原型链。
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
- 给原型添加方法的代码一定要放在替换原型的语句之后，就是`Person.prototype = new Animal()`之后。
- 在通过原型链实现继承时，不能使用对字面量创建原型的方法，不然会重写原型链，导致出错。

## 1.5.原型链的问题

**1.5.1.问题一：实例中改变属性，在原型中也会被改变**
```javascript
function Animal(){
	this.num = ["1", "2", "3"];
}
function Person(){
}
Person.prototype = new Animal();	//实现继承

var Jack = new Person();	//生成Jack实例
Jack.num.push("4");
alert(Jack.num);	//"1,2,3,4"

var Tom = new Person();	//生成新的Tom实例
alert(Tom.num);	//"1,2,3,4"
```
由上面的例子看出，在使用Jack改变属性后，新建的Tom也受到影响，这就是原型链的一个很大的问题。

**1.5.2.问题二：在创建子类型时，不能向超类型的构造函数中传递参数。**
所以一般我们不会单独使用原型链。


## 1.6.借用构造函数
为了解决上面1.5中出现的问题，借用构造函数的技术被大佬们广泛使用。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMjI1MDE1NDUsMTQwNjE0MzUwN119
-->