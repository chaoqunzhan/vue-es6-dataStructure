# React VS Vue

### 简介

Vue使用模板系统而不是JSX，使其对现有应用的升级更加容易。



### Virtual DOM

改变真实的DOM状态远比改变一个JavaScript对象的花销要大得多。

Virtual DOM是一个映射真实DOM的JavaScript对象，如果需要改变任何元素的状态，那么是先在Virtual DOM上进行改变，而不是直接改变真实的DOM。

两者的不同就在于计算差异的**算法**上。

**Vue**，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。

**React**，每当应用的状态被改变时，全部子组件都会重新渲染。当然，这可以通过`shouldComponentUpdate`这个生命周期方法来进行控制，但Vue将此视为默认的优化。



**diff算法：**

比较只会在**同层级进行**, 不会跨层级比较，比较顺序按**先序遍历**，把不同的打上标记，放到数组里面去，统一交给patch处理。
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzNTgyNzc2Ml19
-->