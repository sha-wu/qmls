<template>
	<view>
		<view class="goods-item">
			<!-- 左侧盒子 -->
			<view class="goods-item-left">
				<radio @click="radioClickHandler" v-if="showRadio" :checked="item.goods_state" color="#c00000" ></radio>
				<image :src="item.goods_small_logo || defaultPic " class="goods-pic" ></image>
			</view>
			<!-- 右侧盒子 -->
			<view class="goods-item-right">
				<view class="goods-name">{{item.goods_name}}</view>
				<view class="goods-info-box">
					<view class="goods-price">￥{{item.goods_price | tofixed }}</view>
					<uni-number-box @change="numChangeHandler"  v-if="showNum" :min="1" :value="item.goods_count" ></uni-number-box>
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
			},
			showRadio:{
				type:Boolean,
				default:false
			},
			showNum:{
				type:Boolean,
				default:false
			}
		},
		methods:{
			// radio组件的点击事件处理函数
			radioClickHandler(){
				this.$emit('radio-change',{
					goods_id:this.item.goods_id,
					goods_state:!this.item.goods_state
				})
			},
			
			// 监听numberBox 组件数量变化的事件
			numChangeHandler(val){
				// console.log(val)
				this.$emit('num-change',{
					goods_id:this.item.goods_id,
					goods_count: +val
				})
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
		 display: flex;
		 justify-content: space-between;
		 align-items: center;
		 margin-left: 5px;
		 
		 .goods-pic {
			 width: 100px;
			 height: 100px;
			 display: block;
			 margin-right: 5px;
		 }
	 }
	 .goods-item-right {
		 display: flex;
		 flex: 1;
		 flex-direction: column;
		 justify-content: space-between;
		 .goods-name {
			 font-size: 14px;
		 }
		 .goods-info-box {
			 display: flex;
			 justify-content: space-between;
			 align-items: center;
			 
			 .goods-price {
				 font-size: 16px;
				 color: #c00000;
			 }
		 }
	 }
 }
</style>