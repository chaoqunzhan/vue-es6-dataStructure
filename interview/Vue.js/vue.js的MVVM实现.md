# 什么是MVVM模式

```mermaid
graph LR
A[View] --> B[ViewModel]
B --> A
B --> C[Model]
C-->B
```
**View**: View（视图DOM）	
**
ViewModel**: （通讯，观察者，监听View和Model的变化	
**）	

Model**: （数据，接口，javascript对象）

# Vue的基本应用
这主要是一个简单的Vue例子。
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
	<script type='text/javascript' src='https://cdn.jsdelivr.net/npm/vue'></script>
	<meta charset="UTF-8">
	<title>MVVM</title>
</head>
<body>
	<div id='app'>
		<h1>{{title}}</h1>
		<ul>
			<li  v-for="user in users">{{user.name}}</li>
		</ul>
		<input type="text" v-model="name" id="name-input">
		<button v-on:click="addUser">添加用户</button>
	</div>
	<script type='text/javascript'>
	var model = {
		title : "vue.js的MVVM实现",
		users : [{name:"张良"},{name:"刘邦"},{name:"韩信"}]
	}
	
	var vue = new Vue({
		el : "#app",	//view
		data : model,	//model
		methods : {
			addUser : function(){
				this.users.push({name:""});
				this.name = ""
			}
		}
	});
	</script>
</body>
</html>
```


# 属性定义器介绍
Object.defineProperty(obj, prop, descriptor)
|参数/属性名|类型|描述|注解|
|--|--|--|--|
|obj|Object|目标对象|
|prop|String|目标属性| 
|descriptor:|Object|属性描述器|描述该属性的一些特征|
|-configurable|Boolean|是否可再配置|
|-enumerable|Boolean|是否可枚举|为false则属性非高亮不可for in
|-writable|Boolean|是否可写|
|-value|任意类型|属性的值
|-get|Function|取描述符|属性获得时的回调函数
|-set|Function|存描述符|设置属性时的回调函数

**尝试defineProperty的使用**
```javascript
<script>	
	var model = {
		title : "初始标题"
	};
	console.log("title:"+model.title);
	var val = model.title;
	Object.defineProperty(model, "title", {
		get :function(){
			console.log("调用了getter");
			return val;
		},
		set :function(nval){
			console.log("调用了setter");
			val = nval;
		}
	})

	window.setTimeout(function(){
		model.title="改变标题";
		console.log("title:"+model.title);
	},1000)
</script>
```
**输出结果：**
```javascript
title:初始标题
调用了setter
调用了getter
title:改变标题
```

# MVVM的实现
在Vue.js中，采用观察者-订阅者模式来进行双向数据绑定，通过Object.defineProperty()方法来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

首先，观察者会遍历 data 对象的所有属性，每个属性通过调用 `defineReactive` 方法，转换为getter/setter。defineReactive 方法将data的属性转换为访问器属性：  
get时，进行依赖收集，  
set时，如果数据有改变，则进行订阅通知。

**下面就根据vue.js v2.6.10源码来实现MVVM**
*本例中进行了精简，忽略发布者和订阅者的实现*
```javascript
var view = title;
var model = {
	titie: "test title"
};
reactiveMVVM(model,"title", model.title);

function reactiveMVVM(obj, key, val, customSetter){
	//getOwnPropertyDescriptor：获得obj对象的属性定义器
	var property = Object.getOwnPropertyDescriptor(obj, key);
    if (property && property.configurable === false) {
      return
    }

    // cater for pre-defined getter/setters
    var getter = property && property.get;
    var setter = property && property.set;

	Object.defineProperty(obj, key, {
      enumerable: true,
      configurable: true,
      get: function reactiveGetter () {
        var value = getter ? getter.call(obj) : val;
        // if (Dep.target) {
        //   dep.depend();
        //   if (childOb) {
        //     childOb.dep.depend();
        //     if (Array.isArray(value)) {
        //       dependArray(value);
        //     }
        //   }
        //}
        return value
      },
      set: function reactiveSetter (newVal) {
        var value = getter ? getter.call(obj) : val;
        /* eslint-disable no-self-compare */
        if (newVal === value || (newVal !== newVal && value !== value)) {
          return     //如果值没有改变就不通知视图
        }
        /* eslint-enable no-self-compare */
        if (customSetter) {//客户自定义方法
          customSetter();
        }
        // #7981: for accessor properties without setter
        if (getter && !setter) { return }
        if (setter) {
          setter.call(obj, newVal);
        } else {
          val = newVal;
        }
        view.innerText = val;
        // childOb = !shallow && observe(newVal);//wacther
        // dep.notify();
      }
    });
}
```
**控制台测试**
```javascript
>model.title = "我是新标题"		//输入
<·"我是新标题"					//输出
```VM监听V和M的改变

# Vue的基本应用




# 属性定义器介绍





# MVVM的实现



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA1Njk4NjEyMiwxMTkxMzA0MDczXX0=
-->