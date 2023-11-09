<template>
	<view class="my-settle-container">
		<!-- 全选 -->
		<label class="radio" @click="checkAllState" >
			<radio color="#c00000" :checked="isFullCheck"   /><text>全选</text>
		</label>
		<!-- 合计 -->
		
		<view class="amount-box">
			合计: <text class="amount"> ￥{{checkedGoodsAmount}} </text>
		</view>
		
		<!-- 结算按钮  -->
		<view class="btn-settle"> 结算({{checkedCount}}) </view>
	</view>
</template>

<script>
	import {mapGetters , mapMutations} from 'vuex'
	export default {
		name:"my-settle",
		data() {
			return {
				
			};
		},
		
		computed :{
			...mapGetters('m_cart',['checkedCount', 'total', 'checkedGoodsAmount']),
			isFullCheck(){
				return this.checkedCount === this.total
			}
		},
		
		methods: {
			...mapMutations('m_cart',['updateALlGoodsState']),
			checkAllState(){
				this.updateALlGoodsState(!this.isFullCheck)
			}
		}
	}
</script>

<style lang="scss">
 .my-settle-container {
	position: fixed;
	bottom: 0;
	left: 0;
	width: 100%;
	height: 50px;
	background-color: #fff;
	display: flex;
	justify-content: space-between;
	align-items: center;
	font-size: 14px;
	padding-left: 10px;
	.radio{
		display: flex;
		align-items: center;
	} 
	.amount-box {
		.amount {
			color: #c00000;
			font-weight: bold;
		}
	}
	 
	.btn-settle {
		min-width: 100px;
		height: 50px;
		line-height: 50px;
		text-align: center;
		padding: 0 10px;
		color: white;
		background-color: #c00000;
	}
 }
</style>