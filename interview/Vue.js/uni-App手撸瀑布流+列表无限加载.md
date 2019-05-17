
# 简介瀑布流+列表无限加载：

## 瀑布流
瀑布流是很常见的一种布局方式，像一些图片展示网站，购物网站中最为常见。瀑布流将显示区域划分为固定宽度的几栏。可以保持一致的图片或者div展示区域的宽度，自适应其高度，显示元素的顺序从左到右，自上而下排列。显示效果极好，空间利用率高。

![enter image description here](https://raw.githubusercontent.com/chaoqunzhan/vue-es6-dataStructure/master/interview/Vue.js/images/waterfalljd.jpg)

## 列表无限加载
列表无限加载：当滚轮scroll滑到页面的底部，自动触发新的页面请求，加载更多的显示元素，省去了用户的翻页操作，这在各种APP中极为常见，用户体验极佳。

# 实现逻辑：
本例中采用两栏瀑布流布局，分别用数组cardListLeft和cardListRight储存左右两栏要显示的card（card表示显示的元素），cardLeftHeight 和cardRightHeight表示左右两栏的高度；
 每加载一张card，要进行自适应处理，计算显示的高度，然后把card加到栏高更低的一栏中，更新这一栏的高度；
 初始化时，加载4张card，并设置content的高度为客户端屏幕的高度，以实现触底加载更多。
加载更多方法loadMore触发时，触发新的请求，获取接下来的四张card。以此直到
没有更多card，就显示noMore。


# 主要知识点：

## 底部触发加载更多
使用uni-App的[<scroll-view>](https://uniapp.dcloud.io/component/scroll-view)组件
```javascript
<scroll-view class="content" v-bind:style="{height:contentH+'px'}" scroll-y="true" @scrolltolower="loadMore" lower-threshold="10">
```
动态绑定的 style 不支持直接使用 upx。要使用 `uni.upx2px(Number)` 转换为 `px` 后再赋值。
```html
<!-- - 静态upx赋值生效 -->
<view class="test" style="width:200upx"></view>
<!-- - 动态绑定不生效 -->
<view class="test" :style="{width:winWidth + 'upx;'}"></view>
```

## 获取图片的原始高度
使用<image>组件的[@load](https://uniapp.dcloud.io/component/image)方法，当图片载入完毕时，发布到 AppService 的事件名，事件对象event.detail = {height:'图片高度px', width:'图片宽度px'}
```javascript
function(e){
	let oImgW = e.detail.width; //图片原始宽度
	let oImgH = e.detail.height; //图片原始高度
}
```

# 代码实现：

```html
<scroll-view class="content" v-bind:style="{height:contentH+'px'}" scroll-y="true" @scrolltolower="loadMore" lower-threshold="10">
	<!--瀑布流布局start-->
	<view class="new-list">
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
	<!--瀑布流布局end-->
</scroll-view>


```

```javascript
<script>
	export default {
		data() {
			return {
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

# 实现效果：
![enter image description here](https://raw.githubusercontent.com/chaoqunzhan/vue-es6-dataStructure/master/interview/Vue.js/images/waterfall1.JPG)
![enter image description here](https://raw.githubusercontent.com/chaoqunzhan/vue-es6-dataStructure/master/interview/Vue.js/images/waterfall2.JPG)

![enter image description here](https://raw.githubusercontent.com/chaoqunzhan/vue-es6-dataStructure/master/interview/Vue.js/images/waterfall3.JPG)
最后可以发现，总共12张图片，加载了3次，所以有3次loadmore。
到此还是很不错的（得意脸），欢迎交流改进！！！
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA1NjUyMDAxOV19
-->