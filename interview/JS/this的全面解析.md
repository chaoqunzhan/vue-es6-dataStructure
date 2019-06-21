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

**硬绑定：**
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
// 硬绑定的bar 不可能再修
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcxMjg0NTUyM119
-->