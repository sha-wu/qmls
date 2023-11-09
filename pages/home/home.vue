<template>
	<view>
		<!-- 搜索组件 -->
		<view class="search-box">
			<my-search @click="gotoSearch" ></my-search>
		</view>
		<!-- 轮播图区域 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true" >
			<swiper-item v-for="(item,index) in swiperList" :key="index">
				<navigator class="swiper-item" :url=" '/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id " >
					<image :src="item.image_src" alt=""> </image>
				</navigator>
			</swiper-item>
		</swiper>
		
		<!-- 分类导航区域 -->
		<view class="nav-list">
			<view class="nav-item" v-for="(item,index) in navList" :key="index" @click="navClickHandler(item)" >
				<image :src="item.image_src" class="nav-img"></image>
			</view>
		</view>
		
		<!-- 楼层区域区域 -->
		<!-- 楼层的容器  -->
		<view class="floor-list">
			<!-- 每一个item项 -->
			<view class="floor-item" v-for="(item,index) in floorList" :key="index" >
				<!-- 楼层的标题 -->
				<image :src="item.floor_title.image_src" class="floor-itile" ></image>
				<!-- 楼层图片区域 -->
				<view class="floor-img-box">
					<!-- 左侧大图片的位置 -->
					<navigator :url="item.product_list[0].navigator_url" class="left-img-box">
						<image :src="item.product_list[0].image_src"
						 :style="{width:item.product_list[0].image_width + 'rpx'}"
						 mode="widthFix"
						 ></image>
					</navigator>
					<!-- 右侧四个小图片盒子 -->
					<view class="right-img-box">
						<navigator :url="item2.navigator_url" class="right-img-item"
						 v-for="(item2,index2) in item.product_list"
						  :key="index2" v-if="index2 !== 0"
						   >
							<image :src="item2.image_src" :style="{width: item2.image_width + 'rpx' }" mode="widthFix"></image>
						</navigator>
					</view>
				</view>
				
			</view>
		</view>
		
		
		
		
	</view>
</template>

<script>
	import badgeMix from  '@/mixins/tabbar-badge.js'
	
	export default {
		
		mixins:[badgeMix],
		
		data() {
			return {
				// 轮播图数据列表
				swiperList:[],
				// 分类导航的数据
				navList:[],
				// 楼层的数据
				floorList:[]
			};
		},
		
		onLoad() {
			// 调用方法 获取轮播图数据
			this.getSwiperList()
			
			// 调用方法 获取分类导航数据
			this.getNavList()
			
			// 调用方法 获取楼层数据
			this.getFloorList()
		},
		
		methods:{
			// 获取轮播图数据的方法
			async getSwiperList(){
				const {data:res} =  await uni.$http.get('/api/public/v1/home/swiperdata')
				// 如果请求失败
				if(res.meta.status !== 200) return uni.$showMsg()
				this.swiperList = res.message
				// console.log(res)
			},
			// 获取分类导航数据的方法
			async getNavList(){
				const {data: res} = await uni.$http.get('/api/public/v1/home/catitems')
				if(res.meta.status !== 200) return uni.$showMsg()
				this.navList = res.message
				// console.log(res)
			},
			
			// 点击跳转分类功能
			navClickHandler(item){
				if(item.name == '分类'){
					uni.switchTab({
						url:'/pages/cate/cate'
					})
				}
				// console.log(item)
			},
			// 获取首页楼层数据的功能
			async getFloorList(){
				const {data: res} = await uni.$http.get('/api/public/v1/home/floordata')
				if(res.meta.status !== 200) return uni.$showMsg()
				
				// 对数据进行处理
				res.message.forEach(floor =>{
					floor.product_list.forEach( item =>{
						item.navigator_url =  item.navigator_url.replace(/pages/,'subpkg/goods_list')
					})
				})
				this.floorList = res.message
			},
			// 跳转至搜索页面
			gotoSearch(){
				uni.navigateTo({
					url:'/subpkg/search/search'
				})
			}
		}
		
	}
</script>

<style lang="scss">
	
	swiper{
		height: 330rpx;
		.swiper-item,
		image{
			width: 100%;
			height: 100%;
		}
	}
	
	.nav-list{
		display: flex;
		justify-content: space-around;
		margin: 15px 0;
		.nav-img {
			width: 128rpx;
			height: 140rpx;
		}
	}
	
	.floor-itile{
		width: 100%;
		height: 60rpx;
	}
	
	.right-img-box {
		display: flex;
		flex-wrap: wrap;
		justify-content:space-around;
	}
	
	.floor-img-box{
		display: flex;
		padding-left: 10rpx;
	}

	.search-box {
		position: sticky;
		top: 0;
		left: 0;
		z-index: 999;
	}
</style>
