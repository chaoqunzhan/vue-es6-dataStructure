# 前言

许多的语言的继承支持两种方式：接口继承和实现继承。但是在ECMAScript只支持实现继承，而且主要依靠的是原型链来实现的。所以在理解JS继承前，要先了解原型链的概念。

# 1.原型链

## 1.1.构造函数，原型和实例之间的关系
每个构造函数（`Person`）都含有一个原型对象（`Person.prototype`），而原型对象都包含一个指向构造函数的指针（`constructor`），每个实例（`Jack`）都包含一个指向原型对象的指针（`[[prototype]]`）。
![enter image description here](https://qiniu.cqz21.top/%E5%8E%9F%E5%9E%8B%E9%93%BE1.JPG)

原型链就是在上面的基础上，让原型对象`Person Prototype`等于另一个实例（`Animal`），换句话说就是`Person Prototype`作为`Animal`的实例(`Person.prototype = new Animal()`)，以此下去就形成所谓的原型链。
![enter image description here](https://qiniu.cqz21.top/%E5%8E%9F%E5%9E%8B2.JPG)
## 1.2.默认的原型
事实上所有的引用类型都是默认继承了Object，而这也是通过原型链实现的继承，所以在1.1中的例子需要再加上默认的这一环。
![enter image description here](https://qiniu.cqz21.top/%E5%8E%9F%E5%9E%8B3.JPG)
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


# 2.借用构造函数

为了解决上面1.5中出现的问题，借用构造函数的技术被大佬们广泛使用。
```javascript
function Animal(){
	this.num = ["1", "2", "3"];
}
function Person(){
	Animal.call(this);	//继承Animal
}

var Jack = new Person();	//生成Jack实例
Jack.num.push("4");
alert(Jack.num);	//"1,2,3,4"

var Tom = new Person();	//生成新的Tom实例
alert(Tom.num);	//"1,2,3"
```

在这个例子中相比于1.5例子的区别就一句代码`Animal.call(this);`通过使用`call()`或者`apply()/bind()`方法在新创建的`Person`的实例中调用了`Animal`构造函数，从而实现属性的继承，而且实例之间的属性不相关联。

**传递参数：**
```javascript
function Animal(age){
	this.age= age;
}
function Person(){
	Animal.call(this,"18");	//继承Animal
}

var Jack = new Person();	//生成Jack实例
alert(Jack.age);	//"18"
```
**构造函数的问题：**
构造函数的弊端是方法都在构造函数中定义，函数的复用就很难实现了，这也就跟我们使用继承的目的冲突了，因此构造函数也是很少单独使用的。所以要来学习一下，下面的几种继承方式。

# 3.组合继承
组合继承也叫伪经典继承，就和将原型链和借用构造函数技术结合在一起的继承。基本的思路是：使用原型链实现对原型属性和方法的继承，通过借用构造函数来实现对实例属性的继承。                                   
```javascript
function Animal(name){
	this.name= name;
	this.num = ["1", "2", "3"];
}
Animal.prototype.sayName = function(){
	alert(this,name);
};

function Person(name,age){
	Animal.call(this,name);		//继承Animal的属性
	this.age = age;
}
Person.prototype = new Animal();	//继承Animal的方法
Person.prototype.constructor = Animal;
Person.prototype.sayAge = function(){
	alert(this.age);
};

var Jack = new Person("Jack",18);	//生成Jack实例
Jack.num.push("4");
alert(Jack.num);	//"1,2,3,4"
alert(Jack.sayName);	//"Jack"
alert(Jack.sayAge);		//18

var Tom = new Person("Tom",20);		//生成新的Tom实例
alert(Tom.num);		//"1,2,3"
alert(Tom.sayName);		//"Tom"
alert(Tom.sayAge);		//20
```                    

# 4.原型式继承
```javascript
function object(o){
	function F(){}
	F.prototype = o;
	return new F();
}
```
在object中创建一个临时类型的新实例，复制并返回传入的对象。在ES5中对这种继承进行了规范化，引入了`Object.create( )`。
这种继承其实不是很实用，作简单了解就好。

# 5.寄生式继承
寄生式继承的思路和上面的原型式继承很像，或者可以说是在原型式继承的基础上进行了改进。
```javascript
function createPara(para){
	var clone = object(para);	
	clone.sayHi = function(){
		alert("Hi")
	}
	return clone;
}

var Person = {
	name:"Jack";
	friends:["a","b","c"];
}
var anotherPerson = createPara(Person);
anotherPerson.sayHi;	//Hi
```
在使用寄生继承对对象添加函数的时候，由于函数是不可复用的，所以这种方法的效率很低，一般也不单独使用。

# 6.寄生组合式继承
所谓的寄生组合继承，就是通过借用构造函数来继承属性，通过原型链来继承方法。本质上就是使用寄生式继承来继承超类型的原型，再将结果指定给子类型的原型。
```javascript

function inheritPrototype(subType, superType){
    var prototype = object(superType.prototype); //创建对象的副本
    prototype.constructor = subType; //为副本增加constructor属性
    subType.prototype = prototype; //指定对象，将新创建的对象赋值给子类型
}

function Animal(name){
	this.name = name;
	this.friends = ["a","b","c"];
}

Animal.prototype.sayName = function(){
	alert("this.name")
};

function Person(name,age){
	Animal.call(this,name);		//使用借用构造函数来继承属性
	this.age = age;
}

inheritPrototype(Person,Animal);   //使用寄生式继承方法
Person.prototype.sayAge = function(){	//添加新方法
	alert(this.age);
}
```       

寄生组合式继承是引用类型最理想的继承方式，所以也是我们最需要掌握的知识点。                    

# 总结：
目前使用最多的是组合继承，最理想的是寄生组合式继承，而ES6中出现了Class继承也是一种相当方便使用的继承方式。Class继承将在下一篇文章中学习。。。
                                                                                                                       
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY3NTk3MTQ2NiwxNDA2MTQzNTA3XX0=
-->