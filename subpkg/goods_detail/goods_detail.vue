<template>
	<view v-if="goods_info.goods_name" class="goods-detail-container">
		<!-- 轮播图区域 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true" >
			<swiper-item v-for="(item,index) in goods_info.pics" :key="index" >
				<image :src="item.pics_big" @click="preview(index)" ></image>
			</swiper-item>
		</swiper>
		
		<!-- 商品信息区 -->
		<view class="goods-info-box">
			<!-- 商品价格 -->
			<view class="price">￥{{goods_info.goods_price}}</view>
			<!-- 商品主题区域 -->
			<view class="goods-info-body">
				<!-- 商品名字 -->
				<view class="goods-name">{{goods_info.goods_name}}</view>
				<!-- 收藏区域 -->
				<view class="favi">
					<uni-icons type="star" size="18" color="gray" ></uni-icons>
					<text>收藏</text>
				</view>
			</view>
			<!-- 运费 -->
			<view class="yf">快递：免运费</view>
		</view>
		<!-- 富文本区域 -->
		<rich-text :nodes="goods_info.goods_introduce"></rich-text>
		
		<!-- 商品导航组件 -->
		<view class="goods-nav">
			<uni-goods-nav :fill="true"  :options="options" :buttonGroup="buttonGroup"  @click="onClick" @buttonClick="buttonClick" />
		</view>
	</view>
</template>

<script>
	import {mapState, mapMutations, mapGetters } from 'vuex'
	export default {
		computed:{
			...mapState('m_cart',[]),
			...mapGetters('m_cart', ['total'])
		},
		
		watch:{
			total:{
				handler(newval){
					const findResult = this.options.find(x => x.text === '购物车')
					if(findResult) {
						findResult.info = newval
					}
				},
				immediate:true
			}
			
		},
		
		data() {
			return {
				goods_info:{},
				options: [
					{
						icon: 'headphones',
						text: '客服'
					}, 
					{
						icon: 'shop',
						text: '店铺',
						info: 2,
						infoBackgroundColor:'#007aff',
						infoColor:"red"
					}, 
					{
						icon: 'cart',
						text: '购物车',
						info: 0
					},
				],
				buttonGroup: [
					{
						text: '加入购物车',
					    backgroundColor: '#ff0000',
					    color: '#fff'
					},
					{
					    text: '立即购买',
					    backgroundColor: '#ffa200',
					    color: '#fff'
					}
				]
			};
		},
		
		onLoad(options) {
			const goods_id = options.goods_id
			this.getGoodsDetail(goods_id)
		},
		
		methods:{
			
			...mapMutations('m_cart', ['addToCart']),
			
			// 获取详情页数据
			async getGoodsDetail(goods_id){
				const {data: res} = await uni.$http.get('/api/public/v1/goods/detail',{goods_id})
				if(res.meta.status !== 200 ) return uni.$showMsg()
				
				res.message.goods_introduce = res.message.goods_introduce.replace(/<img /g,'<img style="display:block"').replace(/webp/g,'jpg') 
				
				this.goods_info = res.message
			},
			
			preview(index){
				uni.previewImage({
					current:index,
					urls: this.goods_info.pics.map( x => x.pics_big)
				})
			},
			
			
			onClick(e){
				console.log(e)
				if(e.content.text === '购物车'){
					uni.switchTab({
						url:'/pages/cart/cart'
					})
				}
			},
			
			buttonClick(e){
				if(e.content.text === '加入购物车'){
					const goods = {
						goods_id : this.goods_info.goods_id,
						goods_name : this.goods_info.goods_name,
						goods_price : this.goods_info.goods_price,
						goods_count : 1,
						goods_small_logo : this.goods_info.goods_small_logo,
						goods_state : true
						
					}
					
					this.addToCart(goods)
					
				}
			}
			
			
		}
	}
</script>

<style lang="scss">

.goods-detail-container {
	padding-bottom: 50px;
}

swiper {
	height: 750rpx;
	image{
		width: 100%;
		height: 100%;
	}
}

.goods-info-box {
	padding: 10px;
	padding-right: 0;
	.price{
		color: #c00000;
		font-size: 18px;
		margin: 10px 0;
	}
	.goods-info-body{
		display: flex;
		justify-content: space-between;
		align-items: center;
		.goods-name{
			font-size: 14px;
			margin-right: 10px;
		}
		.favi{
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			border-left: 1px solid #ccc;
			width: 120px;
			font-size: 12px;
			color: gray;
		}
	}
	.yf{
		font-size: 12px;
		color: gray;
		margin: 10px 0;
	}
}

.goods-nav {
	position: fixed;
	bottom: 0;
	left: 0;
	width: 100%;
	z-index: 999;
}

</style>
