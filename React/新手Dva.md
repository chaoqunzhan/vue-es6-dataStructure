# 新手Dva

在来腾讯实习之前都没有React项目经验，更不用说`Dva`了，反正一过来就要上手“小马分析”这种使用`Dva`开发的大型项目，我真的是懵逼的。不过还好有导师和同事的引导和帮助，现在算是差不多了解了`Dva`的所以然。

​				

### 0.瞎**说：

`D.VA`，是网络游戏《守望先锋》中的英雄，（前）职业玩家、机甲驾驶员。隶属于韩国陆军特别机动部队。一个阿里的前端开源框架竟然用网易游戏的游戏人物来命名，只能说技术无界限。



### 1.数据流向：

![](C:\Users\tabzhan\Desktop\PPrerEAKbIoDZYr.png)

简单概括为，`Dva`所有的数据都放在`State`上，而且数据的改变途径只有一种，那就是通过 `dispatch` 发起一个 `action`。只不过在`model`层有两种不同的处理方式，其一：同步数据改变行为通过`Reducer`改变`State`；其二：异步行为通过`Effects`改变`State`。当然还有`Subscription`，用来订阅数据源，根据数据源的改变来触发`dispatch`。

我觉得在这部分就要来说说`connect`，这样就能把整个图讲清楚了。`connect`其实就是`model`和组件连接的桥梁，和`react-redux`中的`connect`一个样。



### 2.Talk is cheap, show me the code:

来结合“小马”的代码（非核心代码），再理解一波。就以我之前完成的用户属性页面为栗子吧。



##### Model

```
export default {
  namespace: 'mta',
  state: {

  },
  reducers: {
    updateState (state, { payload }) {
      return {
        ...state,
        ...payload,
      }
    },
  },
  effects: {
    * getAttrList ({ payload }, { call }) {
      try {
        const { result } = yield call(attrList, payload)
        return result
      } catch (e) {
        message.error(e.message)
        return null
      }
    },
    
    subscriptions: {

  	},
}
```

从`Dva`的核心讲起，通常情况下，一个组件我们就写一个`model`，所以这里是我们用户属性模块对应的`model`。

`namespace`：就是一个命名空间，没什么说的；

`state`： 是状态的初始值，这里没有设置；

`reducers`：用来处理同步数据，其实就是用`updateState`方法来更新一下`state`;

`effects`：这里就是处理异步请求了，`getAttrList`方法通过try…catch…的方式进行后台数据请求，这里应用了`Dva`提供的基础的网络请求函数 `utils/resquest.js` ，简直不能再方便。

```
export async function attrList (params) {
  return request({
    url: INTERFACE.MTA_ATTR_LIST,   //后端接口
    method: 'get',
    data: params,
  })
}
```



`subscriptions`：数据订阅在这个栗子中没有用到，所以我找了另一个地方的栗子简单理解一下。这是一个路由订阅的栗子，如果路由跳转到`/portal`就进入`'app/updateState'`的`dispatch`方法中，进行状态更新。

```
subscriptions: {
    setup ({ dispatch, history }) {
      return history.listen(({ pathname }) => {
        if (pathname === '/portal') {
          dispatch({
            type: 'app/updateState',
            payload: {
              isFullScreen: false,
            },
          })
        }
      })
    },
},
```



##### 组件（`UserAttribute`）

```
  //获得用户属性的数据
  getAttrList = async () => {
    const result = await this.props.dispatch({
      type: 'mta/getAttrList',
      payload: {
        portal_id: this.props.match.params.id,
      },
    })
  }
```

这里是一个在我们的组件中的获取用户数组的函数，就是通过`dispatch`发起的`Action`然后就到model层了。



##### connect

```
export default connect(({ mta, app }) => ({ mta, app }))(UserAttribute)
```

有了`model`和组件，最后的关键就是`connect`了，上面组件中能用`this.props.dispatch`必须要有`connect`，`connect`把我们定义在`mta`和`app`(也就是model)中的`state`和`history`，`dispatch`方法传递到`UserAttribute`中来。



### 3.大同小异`Vuex`

貌似`Vuex`和`Dva`的思路极为相似，两者在各自对应的框架中扮演的都是状态管理的角色（`Vuex`专门为`Vue`服务，`Dva`服务于`React`）。

##### 大同

`Vue`中的`module`对应`Dva`中的`model`，两者都是对store的划分，为的都是让不同组件有自己的对应的状态管理器，这点对于大型项目分块开发很关键。`namespace`在两种方法上也都得到应用。

同样不能直接改变`state`，`Vuex`中需要通过`mutation`这个唯一的途径来改变`state`，而`Dva`需要`dispatch`一个`Action`。



##### 小异

这个小异可以说是“异步”的“异”，两者主要的区别就是在异步的处理上，`Dva`通过`reducer`来处理同步事件，同时把异步事件写的跟同步事件几乎一致，就是`effect`的使用。

`Vuex`的异步处理需要通过`Action`发起`mutation`，然后间接改变`state`的状态。`Vuex`中的`Action`需要使用另一个`API--dispatch`接口来调用。它可以返回`Promise`对象，供调用者进行后续处理。



### redux

状态管理：把组件之间需要共享的状态抽出来，遵循特定的约定，统一管理，让状态的变化可预测。

![](https://user-gold-cdn.xitu.io/2018/12/18/167c11c13f77c38c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



1.用户通过View层交互，也可以是接口请求，发出Action;

2.Store调用Reducer,传入原来的State和Action，返回新的state;

3.如果State有了变化就会调用Store.subscribe(listener),然后渲染新的View;





### 异步处理：

Action 发出以后，Reducer 立即算出 State，这叫做同步；Action 发出以后，过一段时间再执行 Reducer，这就是异步。

怎么才能 Reducer 在异步操作结束后自动执行呢？这就要用到新的工具：中间件（middleware）。

把异步处理放在发起Action的时候。

- 操作发起时的 Action
- 操作成功时的 Action
- 操作失败时的 Action

##### redux-thunk

利用Action Creator（动作生成器），返回一个函数，发出一个操作开始的Action，等到异步操作后，再发起一个操作结束的Action，从而达到处理异步操作的效果。但是Action是有store.dispatch发起的，它的参数不能是函数，只能是对象，所以需要中间件redux-thunk。redux-thunk的作用就是改造store.dispatch，使其能接受函数类型的参数。

##### redux-promise

让Action Creator（动作生成器）返回一个promise对象，在利用redux-promise中间件使得`store.dispatch`方法可以接受 Promise 对象作为参数。其实就是直接用promise来处理异步。



### React-redux:

用来连接React的容器组件（UI呈现）和展示组件（业务逻辑）。简单来说，react-redux 就是多了个 connect 方法连接容器组件和UI组件，这里的“连接”就是一种映射：

- mapStateToProps： 把容器组件的 state 映射到UI组件的 props；
- mapDispatchToProps：把UI组件的事件映射到 dispatch 方法；



### redux-saga:

redux-saga 把异步获取数据这类的操作都叫做副作用（Side  Effect），它的目标就是把这些副作用管理好，让他们执行更高效，测试更简单，在处理故障时更容易。

##### 对比redux-thunk和redux-saga:

###### redux-thunk:

action1(side function)—>redux-thunk监听—>执行相应的有副作用的方法—>action2(plain object)

![](https://user-images.githubusercontent.com/17233651/42163340-a719c8f2-7e34-11e8-9c62-c6270e5eb1cb.png)



###### redux-saga:

action1(plain object)——>redux-saga监听—>执行相应的Effect方法——>返回描述对象—>恢复执行异步和副作用函数—>action2(plain object)

![](https://user-images.githubusercontent.com/17233651/42163582-c92ae632-7e35-11e8-804b-5e6bb60921da.png)







Dva就是把上面的redux,react-redux和redux-saga合在一起，代码完全可以放在一起，写起来轻松愉快！




