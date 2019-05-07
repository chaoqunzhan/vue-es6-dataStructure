
动态绑定的 style 不支持直接使用 upx.
```html
<!-- - 静态upx赋值生效 -->
<view class="test" style="width:200upx"></view>
<!-- - 动态绑定不生效 -->
<view class="test" :style="{width:winWidth + 'upx;'}"></view>
```

使用 `uni.upx2px(Number)` 转换为 `px` 后再赋值。


v-bind:style="{height:contentH+'px'}"
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzEzNDQ3NzMyXX0=
-->