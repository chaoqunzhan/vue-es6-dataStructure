**Cookie**：存储在用户本地终端上的数据主要包括：名字、值、过期时间、路径和域。如果不设置时间，默认关闭消除Cookie。会话Cookie保存在内存里，设置时间的保存在硬盘里。

**session:** 客户端请求创建session时，服务器先检测请求里是否包含了session id，如果存在就说明已为客户端创建过，如果没有就创建。而且session id一般都是保存在Cookie中。

**Cookie和session的区别：**

1、cookie数据存放在客户的浏览器上，session数据放在服务器上。
2、cookie不是很安全，session更安全。
3、session会在一定时间内保存在服务器上，当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用cookie 。
4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
5、session保存在服务器，客户端不知道其中的信心；cookie保存在客户端，服务器能够知道其中的信息 。
6、session中保存的是对象，cookie中保存的是字符串。
7、session不能区分路径，同一个用户在访问一个网站期间，所有的session在任何一个地方都可以访问到，而cookie中如果设置了路径参数，那么同一个网站中不同路径下的cookie互相是访问不到的。

**Web Storage和Cookie的区别：**

1、cookie的大小是受限的，Web Storage更大容量存储。
2、每次请求一个新的页面的时候cookie都会被发送过去，浪费带宽，另外cookie还需要指定作用域，不可跨域调用。Web Storage不会被随着请求发送。
3、cookie的作用是与服务器进行交互，作为http规范的一部分而存在的，而web Storage仅仅是为了在本地“存储”数据而生。
4、Web storage 性能好，获得数据快得多，本地数据可以及时获得。
5、Web storage临时存储：很多时候数据只需要在用户浏览一组页面期间使用，关闭窗口后数据就可以丢弃了，这种情况使用sessionStorage非常方便 。

**Cookie和sessionStorage、localStorage的区别：**

| 特性 |  Cookie|sessionStorage|localStorage
|--|--|--|--|--|
|生命周期  | 生成时就指定maxAge,无指定就默认关闭浏览器时失效 |页面会话期间可用|只能主动清除，否一直存在|
|存放数据大小|4k|一般5M|
|与服务器的通讯|每次请求都要被发送|不被发送|
|易用性|要自己封装setCookie和getCookie|有现成的Api：  localStorage.setItem("user", "Bill Gates") ;localStorage.getItem("user", "maifang51");localStorage.removeItem("user");还有key关键字，用来遍历|
|共同点|都是保存在本地客户端,和session不一样|

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU1MzA1NjU3Nl19
-->