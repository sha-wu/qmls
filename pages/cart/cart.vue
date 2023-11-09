<template>
	<view class="cart-container" v-if="cart.length !== 0 " >

		<!-- 收货地址组件 -->
		<my-address></my-address>

		<!-- 商品列表标题区 -->
		<view class="cart-title">
			<!-- 左侧图标 -->
			<uni-icons type="shop" size="18"></uni-icons>
			<!-- 右侧文本 -->
			<text class="cart-title-text">购物车</text>
		</view>

		<!-- 循环渲染购物车的商品信息 -->
		<uni-swipe-action>
			<block v-for="(item,i) in cart" :key="i">
				<uni-swipe-action-item :right-options="options" @click="swipeItemClickHandler(item)">
					<my-goods :item="item" showNum="true" showRadio="true" @radio-change="radioChangeHandler"
						@num-change="numChangeHandler"></my-goods>
				</uni-swipe-action-item>
			</block>
		</uni-swipe-action>
		
		<!-- 使用自定义的结算组件 -->
		<my-settle></my-settle>

	</view>
	
	<!-- 空白购物车区域 -->
	<view class="empty-cart" v-else>
		<image src="/static/cart-null.png" class="empty-image" ></image>
		<text class="tip-text">空空如也</text>
	</view>
</template>

<script>
	import badgeMix from '@/mixins/tabbar-badge.js'
	import {
		mapState,
		mapMutations
	} from 'vuex'
	export default {

		mixins: [badgeMix],

		computed: {
			...mapState('m_cart', ['cart'])
		},

		data() {
			return {
				options: [{
					text: '删除',
					style: {
						backgroundColor: '#c00000'
					}
				}]
			};
		},

		methods: {
			...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount', 'removeGoodsById']),

			radioChangeHandler(e) {
				this.updateGoodsState(e)
			},

			numChangeHandler(e) {
				this.updateGoodsCount(e)
			},

			swipeItemClickHandler(goods) {
				this.removeGoodsById(goods.goods_id)
			},




		}
	}
</script>

<style lang="scss">
	
	.cart-container {
		padding-bottom: 50px;
	}
	
	.cart-title {
		display: flex;
		align-items: center;
		height: 40px;
		padding-left: 5px;
		border-bottom: 10x solid #efefef;

		.cart-title-text {
			font-size: 14px;
			margin-left: 10px;
		}

	}
	
	.empty-cart {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding-top: 150px;
		.empty-image{
			// width: 100%;
			// height: 100px;
		}
		.tip-text{}
	}
</style>
