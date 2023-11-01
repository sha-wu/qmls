<template>
	<view>
		<view class="goods-item">
			<!-- 左侧盒子 -->
			<view class="goods-item-left">
				<image :src="item.goods_small_logo || defaultPic " class="goods-pic" ></image>
			</view>
			<!-- 右侧盒子 -->
			<view class="goods-item-right">
				<view class="goods-name">{{item.goods_name}}</view>
				<view class="goods-info-box">
					<view class="goods-price">￥{{item.goods_price | tofixed }}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name:"my-goods",
		data() {
			return {
				// 默认图片路径
				defaultPic:'https://static.hdslb.com/error/very_sorry.png'
				
			};
		},
		
		filters : {
			tofixed(num){
				return Number(num).toFixed(2)
			}
		},
		
		props :{
			item:{
				type:Object,
				default: {}
			}
		}
	}
</script>

<style lang="scss">
 .goods-item {
	 display: flex;
	 padding: 10px 5px;
	 border-bottom: 1px solid #f0f0f0;
	 .goods-item-left {
		 .goods-pic {
			 width: 100px;
			 height: 100px;
			 display: block;
			 margin-right: 5px;
		 }
	 }
	 .goods-item-right {
		 display: flex;
		 flex-direction: column;
		 justify-content: space-between;
		 .goods-name {
			 font-size: 14px;
		 }
		 .goods-info-box {
			 
			 .goods-price {
				 font-size: 16px;
				 color: #c00000;
			 }
		 }
	 }
 }
</style>