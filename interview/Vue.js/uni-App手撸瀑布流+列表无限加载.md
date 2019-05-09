
# 简介瀑布流+列表无限加载：

## 瀑布流
瀑布流是很常见的一种布局方式，像一些图片展示网站，购物网站中最为常见。瀑布流将显示区域划分为固定宽度的几栏。可以保持一致的图片或者div展示区域的宽度，自适应其高度，显示元素的顺序从左到右，自上而下排列。显示效果极好，空间利用率高。

![enter image description here](https://raw.githubusercontent.com/chaoqunzhan/vue-es6-dataStructure/master/interview/Vue.js/images/waterfalljd.jpg)

## 列表无限加载
列表无限加载：当滚轮scroll滑到页面的底部，自动触发新的页面请求，加载更多的显示元素，省去了用户的翻页操作，这在各种APP中极为常见，用户体验极佳。

# 主要知识点：



# 实现逻辑：
本例中采用两栏瀑布流布局，


# 代码实现：

动态绑定的 style 不支持直接使用 upx.
```html
<!-- - 静态upx赋值生效 -->
<view class="test" style="width:200upx"></view>
<!-- - 动态绑定不生效 -->
<view class="test" :style="{width:winWidth + 'upx;'}"></view>
```

使用 `uni.upx2px(Number)` 转换为 `px` 后再赋值。


v-bind:style="{height:contentH+'px'}"


```html
<template>
	<view class="box" >
		<view class="weui-search-bar">
			<view class="weui-search-bar__form">
				<view class="weui-search-bar__box">
				  <icon class="weui-icon-search_in-box" type="search" size="14"></icon>
				  <input type="text" class="weui-search-bar__input" placeholder="请输入查询内容" :value="inputdefault" @input="onKeyInput"/>
				  <view class="weui-icon-clear" wx:if="SearchData.value.length > 0" @click="SearchClear">
					<icon type="clear" size="14"></icon>
				  </view>
				</view>
			</view>
			<view class="weui-search-bar__cancel-btn" @click="SearchConfirm">
				 <text wx:if="SearchData.value.length>0" data-key='search'>搜索</text>
				 <text wx:else data-key='back'>返回</text>
			</view>
		</view>
		
		<scroll-view class="content" v-bind:style="{height:contentH+'px'}" scroll-y="true" @scrolltolower="loadMore" lower-threshold="10">
			<view class="bonner">
				<swiper class="swiper" :indicator-dots=true :autoplay=true :interval=5000 :duration=500>
					<swiper-item>
						<img src="@/static/image/bonner/wxBg.jpg" alt="wxBg" >
					</swiper-item>
					<swiper-item>
						<img src="@/static/image/bonner/qqBg.jpg" alt="qqBg">
					</swiper-item>
					<swiper-item>
						<img src="@/static/image/bonner/weiboBg.jpg" alt="weiboBg">
					</swiper-item>
				</swiper>
			</view>
			<view class="item-mune">
				<navigator url="navigate/navigate?title=navigate" hover-class="navigator-hover">
					<img src="@/static/image/item-mune/shu_1.png" alt=""><p>课本</p>
                </navigator>
				<navigator url="navigate/navigate?title=navigate" hover-class="navigator-hover">
					<img src="@/static/image/item-mune/IT.png" alt=""><p>IT</p>
				</navigator>
				<navigator url="navigate/navigate?title=navigate" hover-class="navigator-hover">
					<img src="@/static/image/item-mune/zihangche.png" alt=""><p>自行车</p>
				</navigator>
				<navigator url="navigate/navigate?title=navigate" hover-class="navigator-hover">
					<img src="@/static/image/item-mune/qita.png" alt=""><p>其他</p>
				</navigator>
			</view>
			<view class="new-list">
<!-- 				<img :src="preLoadImg" alt="" mode="widthFix" width="100%" @load="preImageLoad" v-show="false"> -->
				<view class="list-left">
					<view class="card" v-for="(item,index) in cardListLeft">
						<img :src="item.cardImg" alt="" mode="widthFix" width="100%" @load="onImageLoad">
						<view class="card-text">
							<h2>{{item.cardTitle}}</h2>
							<p>{{item.cardText}}</p>
						</view>
					</view>
				</view>
				<view class="list-right">
					<view class="card" v-for="(item,index) in cardListRight" >
						<img :src="item.cardImg" alt="" mode="widthFix" width="100%" @load="onImageLoad">
						<view class="card-text">
							<h2>{{item.cardTitle}}</h2>
							<p>{{item.cardText}}</p>
						</view>
					</view>
				</view>
			</view>
			<view class="noMore" v-if="showNoMore" >我也是有底线的！！！</view>
		</scroll-view>
		
		<!-- <view class="noMore" v-if="showNoMore" >我也是有底线的！！！</view> -->
	</view>
</template>

```

```javascript
<script>
	import shareNavBar from "../../components/share-nav-bar.vue"

	export default {
		components: {shareNavBar},
		data() {
			return {
				inputdefault:'',
				SearchData:{
					value:'nihao'
				},
				
				allcardList:[{
					cardImg:"../../static/image/sample/sample1.jpg",
					cardTitle:"我是第一张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample4.jpg",
					cardTitle:"我是第二张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample5.jpg",
					cardTitle:"我是第三张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample2.jpg",
					cardTitle:"我是第四张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample3.jpg",
					cardTitle:"我是第五张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample6.jpg",
					cardTitle:"我是第六张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample7.jpg",
					cardTitle:"我是第七张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample8.jpg",
					cardTitle:"我是第八张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample9.jpg",
					cardTitle:"我是第九张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample10.jpg",
					cardTitle:"我是第十张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample11.jpg",
					cardTitle:"我是第十一张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				},{
					cardImg:"../../static/image/sample/sample12.jpg",
					cardTitle:"我是第十二张图片",
					cardText:"来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊来见识我的瀑布流啊"
				}],
				cardListLeft:[],		//用来储存左栏的card
				cardListRight:[],		//用来储存右栏的card
				cardLeftHeight:0,
				cardRightHeight:0,		//分别是左右栏的高度
				cardListItem:0,			//作为card的ID
				rImgH:0,				//实际载入的card的高度
				contentH:800,			//设置客户端的屏幕高度默认值
				loadMoreTemp:1,			//作为scoll滚动触底，加载更多的标记，防止多次出发事件，1为允许，0为阻止
				showNoMore:false		//控制底部“我也是有底线view的显示”
			}
		},
		
		onLoad() {
			this.waterfall();			//初始化瀑布流
		},
		methods: {
			SearchClear(){
				this.inputdefault="";
			},
			SearchConfirm(){
				console.log("搜索："+this.SearchData.value);
			},
			onKeyInput: function(event) {
				this.SearchData.value = event.target.value
			},
			
			
			onImageLoad: function(e){

				let divWidth = 345;			//实际显示的单栏宽度，345upx
				let oImgW = e.detail.width; //图片原始宽度
				let oImgH = e.detail.height; //图片原始高度
				let rImgH = divWidth*oImgH/oImgW+170;	//重新计算当前载入的card的高度
				
				if(this.cardListItem==0){
					this.cardLeftHeight += rImgH;	//第一张card高度加到cardLeftHeight
					this.cardListItem++;			//card索引加1
					this.cardListRight.push(this.cardList[this.cardListItem]);	//添加第二张card到cardListRight数组
				}else{
					this.cardListItem++;		//card索引加1
					
						if(this.cardLeftHeight > this.cardRightHeight){		//把card的高度加到目前高度更低的栏中
							this.cardRightHeight += rImgH;		//第二张card高度加到cardRightHeight
						}else{
							this.cardLeftHeight += rImgH;
						}
						
					if(this.cardListItem<this.cardList.length){				//根据目前的栏高，把下一张card，push到低的那栏
						if(this.cardLeftHeight > this.cardRightHeight){
							this.cardListRight.push(this.cardList[this.cardListItem]);		//添加第三张card到cardListRight数组
						}else{
							this.cardListLeft.push(this.cardList[this.cardListItem]);
						}
					}
				}
				
				// console.log(+this.cardListItem);
				if(this.cardListItem%4 == 0){				//每次载入的card数量设置为4，只有载入完成才允许下一次的scroll触底，触发loadMore
					this.loadMoreTemp = 1;
				}
				
				
			},
			
			waterfall: function(){
				this.cardList = this.allcardList.slice(0,4);		//初始化图片显示
				this.cardListLeft.push(this.cardList[0]);
				this.preLoadImg = this.cardList[0].cardImg;
				var that = this;
				uni.getSystemInfo({		//利用uni-APP获取系统信息Api，获取客户端的屏幕高度，设置成scoll-view的高度，实现触底事件
					success: function (res) {
						that.contentH = res.windowHeight;
					},
				});
			},
			
			loadMore: function(){
				if(this.loadMoreTemp == 1){			//loadMoreTemp==1,才允许触发
					console.log("loadMore");
					this.loadMoreTemp = 0;			//防止多次触发
					
					let newcardList = this.allcardList.slice(this.cardListItem,this.cardListItem+4);//模拟后端接口返回四个新的数据
					
					//console.log(newcardList);
					if(!newcardList.length == 0){				//判断是否还有新数据
						this.cardList = this.cardList.concat(newcardList);			//返回的新数据加到当前的cardList
						if(this.cardLeftHeight > this.cardRightHeight){				//把第一个新数据加到目前更低的栏上，以触发@load="onImageLoad"
							this.cardListRight.push(newcardList[0]);			
						}else{
							this.cardListLeft.push(newcardList[0]);
						}
					}else{
						this.showNoMore = true;				//没有新数据就显示到底了
					}
					
					
				}
				
			}
		}
	}
</script>

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzI3MjM3MjQ5XX0=
-->