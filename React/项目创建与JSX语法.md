# 一、简介：

**React** 是一个用于构建用户界面的 JavaScript 库。
React 起源于 Facebook 的内部项目，用来架设 Instagram 的网站，并于 2013 年 5 月开源。

**特点：**
-   **声明式设计**  −React采用声明范式，可以轻松描述应用。
    
-   **高效**  −React通过对DOM的模拟，最大限度地减少与DOM的交互。
    
-   **灵活**  −React可以与已知的库或框架很好地配合。
    
-   **JSX**  − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。
    
-   **组件**  − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。
    
-   **单向响应的数据流**  − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。






```html
<div id="app"></div>
```

```javascript
<script>
    /**
     * ReactDOM.render(要渲染的内容，放置内容的容器)
     *     ReactDOM：React框架提供的一个对象
     *     容器不能使body
     */

    ReactDOM.render(
        'Hello React!',
        // document.body
        document.getElementById('app')
    );
</script>
 ```




```html
<div id="app"></div>
```

```javascript
    <script>
        /**
         * ReactDOM.render(要渲染的内容，放置内容的容器)
         *     ReactDOM：React框架提供的一个对象
         *     容器不能使body
         */

        ReactDOM.render(
            '<h1>Hello React!</h1>',    //这里的内容不会作为html进行解析，只会作为普通的字符串内容
            // document.body
            document.getElementById('app')
        );
    </script>
```

```html
<div id="app"></div>
```

```javascript
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

```html
<div id="app"></div>
```

```javascript
    <script type="text/babel">

        /**
         * JSX
         *     允许在js中书写xml
         *     xml中可以包含子元素，但是结构中只能有且仅有一个顶级元素
         *     支持插值表达式
         *         插值表达式：类似ES6中模板字符串 ${表达式}
         *         插值表达式语法：{表达式}
         */

        ReactDOM.render(
            <div>
                <h1>Hello React!</h1>
                <h1>Hello React!</h1>
                <h1>{1+2}</h1>
            </div>,
            document.getElementById('app')
        );
    </script>
```

```html
<div id="app"></div>
```

```javascript
    <script type="text/babel">

        /**
         * 插值表达式
         *     在xml中插入一个表达式（得到一个结果：值）
         *
         *     字符串
         *     数字
         *
         *     下面三种类型的值输出空字符（不会报错）
         *     布尔值
         *     空
         *     未定义
         *
         *     插值表达式中不能直接输出对象
         *      对象
         *     但是，如果是一个数组对象则是可以的，需要注意的是，React对数组进行转字符串操作，并且是使用空字符串进行连接：arr.join('')
         */

        ReactDOM.render(
            <div>
                <h1>Hello React!</h1>
                <h1>Hello React!</h1>
                <h1>{'Hello'}</h1>
                <h1>{1}</h1>
                <h1>{true}</h1>
                <h1>{null}</h1>
                <h1>{undefined}</h1>
                {/*<h1>{ {left: 100} }</h1>*/}
                { [1,2,3] }
            </div>,
            document.getElementById('app')
        );
    </script>
```


```html
<div id="app"></div>
```

```javascript
    <script type="text/babel">

        /**
         * JSX
         *     属性
         *     jsx标签也是可以支持属性设置的
         *     基本使用和html/XML类型，在标签上添加属性名 = 属性值，值必须使用""包含
         *     值是可以接收插值表达式的
         *
         *     注意：
         *         1. class：使用className属性来代替
         *         2. style：值必须使用对象
         */

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


```html
<div id="app"></div>
```

```javascript
    <script type="text/babel">

        /**
         * React没有模板语法，插值表达式中只支持表达式，不支持语句：for，if
         *
         */

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
                        /*['left', 'top']*/
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


```html
<div id="app"></div>
```

```javascript
    <script type="text/babel">

        var users = ['刘伟','莫涛','钟毅'];
        var users2 = ['张三','李四','王五'];

        /**
         * 函数复用
         *     组件：拥有独立功能的一个模块
         * React还提供组件的另外一种使用的方式：标签化
         * 标签化：传参，通过标签属性传入的
         */

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



```html
<div id="app"></div>
```

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

        /**
         * 如果一个函数或类作为一个组件去使用的话，那么名称必须首字母大写
         * 如果使用类实现组件，那么需要继承一个父类：React.Component
         * 组件类必须实现一个render()的方法
         * props：传入的参数必须是用对象下的一个属性props来接收
         */

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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExODU3NzI2NzAsLTIwODg3NDY2MTJdfQ
==
-->