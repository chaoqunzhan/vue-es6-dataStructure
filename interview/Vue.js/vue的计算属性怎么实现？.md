**计算属性和方法的区别：**
1  、计算属性是基于它所依赖的数据进行更新，在有在相关依赖的数据发生变化了才会进行更新，而普通的方法每次都要执行。
2 、 计算属性是有缓存的，只要它所依赖的数据没有发生改变，后面的每一次访问计算属性中的值，都是之前缓存的结果，不会重复执行。

代码演示

    <div id="example">
    	<input v-model="message" placeholder="edit me" style = "font-size:40px">
    	<p>Original message: "{{ message }}"</p>
    	<p>Computed reversed message: "{{ computedMessage }}"</p>
    	<p>methods reversed message: "{{ methodMessage }}"</p>
    </div>
    <script>
    	var vm = new Vue({
    		el: '#example',
    		data: {
    			message: 'Hello',
    		},
    		computed: {
    			// 计算属性的 getter
    			computedMessage: function () {
    				// `this` 指向 vm 实例
    				return this.message.split('').reverse().join('');
    			}
    		},
    		methods:{
    			methodMessage:function(){
    			this.Message = this.message.split('').reverse().join('')
    		}
    		}
    	})
    </script>

**计算属性的实现过程：**
1、首先 message属性会被处理为存取器属性，访问 message就会触发其 get 函数;
2、处理属性computedMessage 的时候会触发this.message，从而触发它的get;
3、message 的 get 函数会添加 message属性的依赖项，而刚才在处理计算属性过程中，computedMessage 已经作为依赖项被传给了一个全局变量，message 的 get 函数会检测到这个全局变量，并将其添加到自身的订阅者列表中;
4、对 message 赋予新的值时，会触发其 set 函数，set 函数中会遍历执行订阅者，computedMessage 的值就是在这个时候更新的。

一知半解，还要继续研究。。。

参考： [Vue.js 计算属性的秘密](https://www.cnblogs.com/kidney/p/7384835.html?utm_source=debugrun&utm_medium=referral)
参考：[计算属性的工作原理](https://skyronic.com/blog/vuejs-internals-computed-properties)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUyOTQwNjc0Ml19
-->