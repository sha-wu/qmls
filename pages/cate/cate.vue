<template>
	<view>
		<my-search @click="gotoSearchdetil()" ></my-search>
		<view class="scroll-view-container">
			<!-- 左侧滑动区域 -->
			<scroll-view class="left-scroll-view"
			 scroll-y="true" style="height: 100vh"
			  >
				<block v-for="(item,index) in cateList" :key="index" >
					<view class="left-scroll-view-item"
					 :class= "index === active? 'active':'' " 
					 @click="activeChanged(index)">{{item.cat_name}}</view>
				</block>
			</scroll-view>
			<!-- 右侧滑动区域 -->
			<scroll-view scroll-y="true" style="height: 100vh;" :scroll-top="scrollTop" >
				<!-- 二级分类的标题 -->
				<view class="cate-lv2" v-for="(item2,i2) in cateLevel2" :key="i2" >
					<view class="cate-lv2-title" >/{{item2.cat_name}}/ </view>
					<!-- 二级分类下的三级分类列表 -->
					<view class="cate-lv3-list">
						<!-- 三级分类的item项 -->
						<view class="cate-lv3-item"   v-for="(item3,i3) in item2.children" :key="i3" @click="gotoGoodsList(item3)" >
							<!-- 三级分类的图片 -->
							<image :src="item3.cat_icon"></image>
							<!-- 三级分类的文本 -->
							<text>{{item3.cat_name}}</text>
						</view>
					</view>
				</view>
				
				
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 分类列表数据
				cateList:[],
				// 用来判断是否添加类名
				active:0,
				// 存放二级分类数据
				cateLevel2:[],
				// 滚动的位置
				scrollTop: 0
			};
		},
		onLoad() {
			this.getCateList()
		},
		methods:{
			async getCateList(){
			 	const {data:res} =  await uni.$http.get('/api/public/v1/categories')
				if(res.meta.status !== 200) return uni.$showMsg()
				// console.log(res)
				this.cateList = res.message
				// console.log(this.cateList)
				this.cateLevel2 =  this.cateList[0].children
				// this.cateLevel3 =  this.cateLevel2[0].children
			},
			
			// 导航的切换 和 重新赋值二级菜单 并把滚动距离设置为0
			activeChanged(index){
				this.active = index
				this.cateLevel2 = this.cateList[index].children
				this.scrollTop = this.scrollTop === 0 ? 1 : 0;
			},
			
			// 跳转至商品列表页
			gotoGoodsList(item){
				uni.navigateTo({
					url:'/subpkg/goods_list/goods_list?cid=' + item.cat_id
				})
			},
			gotoSearchdetil(){
				uni.navigateTo({
					url:'/subpkg/search/search'
				})
			}
			
		}
	}
</script>

<style lang="scss">
	.scroll-view-container {
		display: flex;
		.left-scroll-view{
			width: 120px;
		}
		.left-scroll-view-item{
			background-color: #f7f7f7;
			line-height: 60px;
			text-align: center;
			font-size: 12px;
			&.active {
				position: relative;
				top: 0;
				left: 0;
				background-color: #fff;
				&::before {
					content: '';
					display: block;
					position: absolute;
					top: 50%;
					left: 0;
					transform: translateY(-50%);
					width: 3px;
					height: 30px;
					background-color: #c00000;				
				}
			}
		}
		.cate-lv2-title{
			font-size: 12px;
			font-weight: bold;
			text-align: center;
			padding: 15px 0;
		}
		.cate-lv3-list {
			display: flex;
			flex-wrap: wrap;
			.cate-lv3-item{
				width: 33.3%;
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;
				margin-bottom: 10px;
				image{
					width: 60px;
					height: 60px;
				}
				text{
					font-size: 12px;
				}
			}
		}
	}
</style>
