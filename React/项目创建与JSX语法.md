# 一、简介：

**React** 是一个用于构建用户界面的 JavaScript 库。
React 起源于 Facebook 的内部项目，用来架设 Instagram 的网站，并于 2013 年 5 月开源。

## 1.1.特点:

   **声明式设计**  −React采用声明范式，可以轻松描述应用。
    
-   **高效**  −React通过对DOM的模拟，最大限度地减少与DOM的交互。
    
-   **灵活**  −React可以与已知的库或框架很好地配合。
    
-   **JSX**  − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。
    
-   **组件**  − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。
    
-   **单向响应的数据流**  − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。

## 1.2.React的安装：

和普遍的JS库一样，React支持多种安装方式。

**1.2.1.添加`<Script>`标签:**
```javascript
  <!-- ... 其它 HTML ... -->

  <!-- 加载 React。-->
  <!-- 注意: 部署时，将 "development.js" 替换为 "production.min.js"。-->
  <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>

  <!-- 加载我们的 React 组件。-->
  <script src="like_button.js"></script>
</body>
```
**1.2.2.通过 npm (cnpm)使用React**
```javascript
$ npm install -g cnpm --registry=https://registry.npm.taobao.org 
$ npm config set registry https://registry.npm.taobao.org
```
当然这种方法的前提是在node.js和npm环境下才能使用。

**1.2.3.使用create-react-app构建React开发环境**
```javascript
$ cnpm install -g create-react-app
$ create-react-app my-app
$ cd my-app/ 
$ npm start
```
这就像`vue`中的`vue-cli`，是一个快速构建项目的工具，自动生成项目的目录，提高开发效率。

# 二、结合实例打基础

## 2.1.Hello World

```html
<div id="app"></div>
```

```javascript

<script>
    ReactDOM.render(
        'Hello React!',
        // document.body
        document.getElementById('app')
    );
</script>
 ```

- ReactDOM.render(要渲染的内容，放置内容的容器)；
- ReactDOM：React框架提供的一个对象；
- 放置内容的容器不能是body；



## 2.2.JSX语法

```javascript
<script>
    ReactDOM.render(
        '<h1>Hello React!</h1>',    //这里的内容不会作为html进行解析，只会作为普通的字符串内容
        document.getElementById('app')
    );
</script>
```
如果`ReactDOM.render()`的内容中出现`html`标签，是不会被解析成`html`的，只会被解析成字符串。这就给我们带来了很大的不方便，所以需要引入**JSX**。

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，比如，在 UI 中需要绑定处理事件、在某些时刻状态发生变化时需要通知到 UI，以及需要在 UI 中展示准备好的数据。

React 并没有采用将【标记与逻辑进行分离到不同文件】这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中。

React使用 JSX，但是大多数人发现，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。它还可以使 React 显示更多有用的错误和警告消息。

```javascript
//使用JSX,需要引入babel包用来解析相关语法
<script src="js/babel.min.js"></script>

<script type="text/babel">
    // type="text/babel" : 浏览器不会解析这里的代码，babel就会读取到这里的内容，并进行解析，然后生成浏览器可识别的js代码
    /**
     * JSX
     *     JavaScript XML
     *     基于JavaScript的语言，融合了XML,我们可以在js中书写xml
     *
     *     xml: 可扩展（自定义）标记语言
     *     <users>
     *         <user id="1">
     *             <username>MoTao</username>
     *         </user>
     *         <user>
     *             <username>MoTao</username>
     *         </user>
     *     </users>
     */

    ReactDOM.render(
        <h1>Hello React!</h1>,
        document.getElementById('app')
    );
</script>
```

## 2.3.插值表达式
JSX语法是允许使用插值表达式的，但是对于几种基本数据类型的插值表达式，有所不同。
- 插值表达式：类似ES6中模板字符串 `${表达式}`；
- 插值表达式语法：`{表达式}`；

```javascript
<script type="text/babel">
    ReactDOM.render(
        <div>
            <h1>Hello React!</h1>
            <h1>{'Hello'}</h1>	//字符串
            <h1>{1}</h1>	//数值
            <h1>{true}</h1>		//布尔型
            <h1>{null}</h1>		//空
            <h1>{undefined}</h1>	//未定义
            {/*<h1>{ {left: 100} }</h1>*/}		//对象
            { [1,2,3] }		//数组对象
        </div>,
        document.getElementById('app')
    );
</script>
```
- 字符串：正常显示；
- 数字：正常显示；

- 布尔值：不宣示，不报错；
- 空：不宣示，不报错;
- 未定义：不宣示，不报错;

- 对象：插值表达式中不能直接输出对象；但是，如果是一个数组对象则是可以的，需要注意的是，React对数组进行转字符串；操作，并且是使用空字符串进行连接：`arr.join('')`;


## 2.4.属性

```javascript
<script type="text/babel">
    var a = 'miaov';

    ReactDOM.render(
        <div>
            <h1 id="a">Hello React!</h1>
            <h1 id={a}>Hello React!</h1>
            <h1 className="box">Hello React!</h1>
            <h1 style={ {color: 'red'} }>Hello React!</h1>
        </div>,
        document.getElementById('app')
    );
</script>
```

**JSX**标签也是可以支持属性设置的；
基本使用和`html/XML`类型，在标签上添加属性名 = 属性值，值必须使用`""`包含
值是可以接收插值表达式的.

注意：
- class：使用className属性来代替；
- style：值必须使用对象；



## 2.5.列表输出
React没有模板语法，插值表达式中只支持表达式，不支持语句：for，if；
```javascript
<script type="text/babel">
    var users = ['刘伟','莫涛','钟毅'];
    var obj = {left: 100, top: 200};

    ReactDOM.render(
        <div>
            <ul>
                {
                    /*
                    根据数组中的内容生成一个包含有结构的新数组
                    通过数组生成的结构，每一个元素必须包含有一个key属性，同时这个key属性的值必须是唯一的
                     */
                    users.map( (user, index) => {
                        return <li key={index}>{user}</li>
                    } )
                }
            </ul>

            <ul>
                {
                    Object.keys(obj).map( key => {
                        return <li key={key}>{key} - {obj[key]}</li>
                    } )
                }
            </ul>
        </div>,
        document.getElementById('app')
    );
</script>
```
**key** 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

## 2.6.组件复用

```javascript
<script type="text/babel">

    var users = ['刘伟','莫涛','钟毅'];
    var users2 = ['张三','李四','王五'];

    function List(props) {

        /**
         * props:接受来自标签上的所有属性
         *     props = {
         *         data: users,
         *         a: '1'
         *     }
         */

        return props.data.map( ( item, index ) => {
            return <li key={index}>{item}</li>
        } )
    }

    ReactDOM.render(
        <div>
            <ul>
                {/*List(users)*/}
                <List data={users} a="1" />
            </ul>

            <ul>
                {/*List(users2)*/}
                <List data={users2} />
            </ul>
        </div>,
        document.getElementById('app')
    );
</script>
```
- 组件就是拥有独立功能的一个模块，这里的例子是采用函数复用，作简单了解；
- React还提供组件的另外一种使用的方式：标签化；
- 标签化：传参，通过标签属性传入的；


## 2.7.组件化——类

```javascript
<!-- <Panel>
    <Group>
        <List></List>
    </Group>
    <Group>
        <List></List>
    </Group>
    <Group>
        <List></List>
    </Group>
</Panel> -->

<script type="text/babel">

    var users = ['刘伟','莫涛','钟毅'];
    var users2 = ['张三','李四','王五'];

    class List extends React.Component {

        render() {
            return this.props.data.map( (item, index) => {
                return <li key={index}>{item}</li>
            } )
        }

    }

    ReactDOM.render(
        <div>
            <ul>
                <List data={users} />
            </ul>

            <ul>
                <List data={users2} />
            </ul>
        </div>,
        document.getElementById('app')
    );
</script>
```

- 如果一个函数或类作为一个组件去使用的话，那么名称必须首字母大写；
- 如果使用类实现组件，那么需要继承一个父类：`React.Component`；
- 组件类必须实现一个`render()`的方法；
- `props`：传入的参数必须是用对象下的一个属性props来接收；


# **未完待更。。。**

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY1NDA3MDYxLC0yMDg4NzQ2NjEyXX0=
-->