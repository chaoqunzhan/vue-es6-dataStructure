# 一、调用位置
this的调用位置就是函数在代码中被调用的位置（不是声明的位置）。分析调用位置最重要的就是分析调用栈。下面一个简单例子，理解调用栈和调用位置：
```javascript
function baz() {
	// 当前调用栈是：baz
	// 因此，当前调用位置是全局作用域
	console.log( "baz" );
	bar(); // <-- bar 的调用位置
}
function bar() {
	this全面解析 ｜ 83
	// 当前调用栈是baz -> bar
	// 因此，当前调用位置在baz 中
	console.log( "bar" );
	foo(); // <-- foo 的调用位置
}
function foo() {
	// 当前调用栈是baz -> bar -> foo
	// 因此，当前调用位置在bar 中
	console.log( "foo" );
}
baz(); // <-- baz 的调用位置
```
上例子中分析出了函数的调用位置，也就确定this的调用位置。

# 二、绑定规则

## 2.1.默认绑定

下面例子中的`foo()`是直接使用不带任何修饰的函数引用进行调用的，因此只能使用`默认绑定`。
```javascript
function foo() {
	console.log( this.a );
}
var a = 2;
foo(); // 2
```

然鹅，在严格模式（`strict mode`）下，全局对象无法使用默认绑定，因此下面的例子中，`this`会绑定到`undefined`。
```javascript
function foo() {
	console.log( this.a );
}
var a = 2;
(function(){
	"use strict";
	foo(); // 2
})();
```

## 2.2.隐式绑定
隐式绑定会把函数调用中的`this`绑定到上下文对象。下面的例子中的`foo()`的`this`绑定在`obj2`上，所以输出是`42`.
```javascript 
function foo() {
	console.log( this.a );
}
var obj2 = {
	a: 42,
	foo: foo
};
var obj1 = {
	a: 2,
	obj2: obj2
};
obj1.obj2.foo(); // 42
```

**隐式丢失**
一个最常见的`this`绑定问题就是被隐式绑定的函数会丢失绑定对象，也就是说它会应用默认绑定，从而把`this` 绑定到全局对象或者`undefined` 上。
```javascript
function foo() {
	console.log( this.a );
}
var obj = {
	a: 2,
	foo: foo
};
var bar = obj.foo; // 函数别名！
var a = "oops, global"; // a 是全局对象的属性
bar(); // "oops, global"
```
上例中，虽然`bar` 是`obj.foo `的一个引用，但是实际上，它引用的是`foo`函数本身，因此此时的`bar()` 其实是一个不带任何修饰的函数调用，因此应用了**默认绑定**。

```javascript
function foo() {
	console.log( this.a );
}
function doFoo(fn) {
	// fn 其实引用的是foo
	fn(); // <-- 调用位置！
}
var obj = {
	a: 2,
	foo: foo
};
var a = "oops, global"; // a 是全局对象的属性
doFoo( obj.foo ); // "oops, global"
```
上例中的参数传递其实就是一种隐式赋值，所以结果跟前面的例子一样。

## 2.3.显式绑定
`JavaScript`有函数都可以使用`call(..)`和`apply(..)` 方法，进行显式绑定`this`。`call(..)`和`apply(..)`的区别就在于传入参数形式不同，
```javascript
function foo() {
	console.log( this.a );
}
var obj = {
	a:2
};
foo.call( obj ); // 2
```

**2.3.1.硬绑定：**
硬绑定可以用来解决上面说到的**隐式丢失**问题。下面的例子中，实际上是在每次调用`bar`函数的时候都执行了硬绑定，所以可以解决上述的问题。
```javascript
function foo() {
	console.log( this.a );
}
var obj = {
	a:2
};
var bar = function() {
	foo.call( obj );
};
bar(); // 2
setTimeout( bar, 100 ); // 2
// 硬绑定的bar 不可能再修改
bar.call( window ); // 2
```

由于硬绑定是一种很常用的放大，所以在ES5中内置了`bind()`方法，作为辅助绑定方法。
```javascript
function foo(something) {
	console.log( this.a, something );
	return this.a + something;
}
var obj = {
	a:2
};
var bar = foo.bind( obj ); //this硬绑定在obj上
var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

**2.3.2.API调用的“上下文”**
第三方库的许多函数，以及JavaScript 语言和宿主环境中许多新的内置函数，都提供了一个可选的参数，通常被称为“上下文”（context），其作用和`bind(..)` 一样，确保你的回调函数使用指定的this。
```javascript
function foo(el) {
	console.log( el, this.id );
}
var obj = {
	id: "awesome"
};
// 调用foo(..) 时把this 绑定到obj
[1, 2, 3].forEach( foo, obj );
// 1 awesome 2 awesome 3 awesome
```
这些函数实际上就是通过`call(..)` 或者`apply(..)` 实现了显式绑定，这样可以少些一些代码。

## 2.4.new绑定
使用new 来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。
1. 创建（或者说构造）一个全新的对象。
2. 这个新对象会被执行[[ 原型]] 连接。
3. 这个新对象会绑定到函数调用的this。
4. 如果函数没有返回其他对象，那么new 表达式中的函数调用会自动返回这个新对象。
```javascript
function foo(a) {
	this.a = a;
}
var bar = new foo(2);
console.log( bar.a ); // 2
```
使用new 来调用`foo(..)` 时，我们会构造一个新对象并把它绑定到`foo(..)` 调用中的`this`上。new 是最后一种可以影响函数调用时this 绑定行为的方法，我们称之为**new 绑定**。

# 三、优先级
下面既代表绑定的优先级，又是我们在实际应用中，判断`this`的顺序。

1. 函数是否在new 中调用（new 绑定）？如果是的话this 绑定的是新创建的对象。
`var bar = new foo()`
2. 函数是否通过call、apply（显式绑定）或者硬绑定调用？如果是的话，this 绑定的是
指定的对象。
`var bar = foo.call(obj2)`
3. 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上
下文对象。
`var bar = obj1.foo()`
4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到undefined，否则绑定到
全局对象。
`var bar = foo()`

# 四、绑定例外
上面介绍了几种绑定方法，但总是有例外，这里就总结一下例外的几种情况。

## 4.1.被忽略的this
如果你把`null` 或者`undefined` 作为`this` 的绑定对象传入`call、apply `或者`bind`，这些值在调用时会被忽略，实际应用的是默认绑定规则。

## 4.2.间接引用
间接引用最容易发生在赋值的时候：
```javascript
function foo() {
	console.log( this.a );
}
var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };
o.foo(); // 3
(p.foo = o.foo)(); // 2
```
赋值表达式`p.foo = o.foo`的返回值是目标函数的引用，因此调用位置是`foo() `而不是`p.foo() `或者`o.foo()`。所以这时候会执行默认绑定，把`this`绑定在全局。

## 4.3.软绑定



# 五、this的词法
我们之前介绍的四条规则已经可以包含所有正常的函数。但是ES6 中介绍了一种无法使用这些规则的特殊函数类型：**箭头函数**。箭头函数不使用`this` 的四种标准规则，而是根据外层（函数或者全局）作用域来决定`this`。

我们来看看箭头函数的词法作用域：
```javascript
function foo() {
	// 返回一个箭头函数
	return (a) => {
	//this 继承自foo()
	console.log( this.a );
	};
}
var obj1 = {
	a:2
};
var obj2 = {
	a:3
};
var bar = foo.call( obj1 );
bar.call( obj2 ); // 2, 不是3 ！
```
`foo()` 内部创建的箭头函数会捕获调用时`foo()` 的`this`。由于`foo()` 的`this` 绑定到`obj1`，`bar`（引用箭头函数）的`this` 也会绑定到`obj1`，箭头函数的绑定无法被修改。（`new` 也不行！）













<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ5NTIwNTE4MV19
-->