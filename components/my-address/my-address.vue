<template>
	<view>
		<!-- 选择收货地址盒子 -->
		<view v-if="JSON.stringify(address) === '{}'" class="address-choose-box">
			<button type="primary" size="mini" class="btnChooseAddress" @click="chooseAddress" >请选择收货地址+</button>
		</view>
		
		<!-- 渲染收货信息的盒子 -->
		<view v-else class="address-info-box" @click="chooseAddress">
			<view class="row1">
				<view class="row1-left">
					<view class="username">收货人：{{address.userName}}</view>
				</view>
				<view class="row1-right">
					<view class="phone">电话：{{address.telNumber}}</view>
					<uni-icons type="arrowright"></uni-icons>
				</view>
			</view>
			<view class="row2">
				<view class="row2-left">收货地址：</view>
				<view class="row2-right">{{addstr}}</view>
			</view>
		</view>
		
		<!-- 底部边框线 -->
		<view class="address-border"></view>
	</view>
</template>

<script>
	import {mapState,mapMutations,mapGetters} from 'vuex'
	export default {
		name:"my-address",
		data() {
			return {
				
				// address:{}
			};
		},
		
		methods: {
			...mapMutations('m_user',['updateAddress']),
			async chooseAddress(){
				const [err,succ] =  await uni.chooseAddress().catch(err => err)
				if(err === null && succ.errMsg === "chooseAddress:ok"){
					// this.address = succ
					this.updateAddress(succ)
				}
				// https://www.bilibili.com/video/BV1834y1676P/?p=158&spm_id_from=pageDriver&vd_source=eafb4f0030fb5780a8345bdcfe130c20
				// cityName: "北京市"
				// countyName: "东城区"
				// detailInfo: "东华门街道123456"
				// detailInfoNew: "123456"
				// errMsg: "chooseAddress:ok"
				// nationalCode: "110101"
				// nationalCodeFull: "110101001"
				// postalCode: "100010"
				// provinceName: "北京市"
				// streetName: "东华门街道"
				// telNumber: "13982614070"
				// userName: "张三"  
			}
		},
		
		computed :{
			
			...mapState('m_user',['address']),
			
			...mapGetters('m_user',['addstr'])
			
		}
	}
</script>

<style lang="scss">
.address-border {
	width: 100%;
	height: 5px;
	background-color: #ffaa00;
}

.address-choose-box {
	display: flex;
	justify-content: center;
	align-items: center;
	height: 90px;
	.btnChooseAddress{
		
	}
}

.address-info-box{
	display: flex;
	flex-direction: column;
	justify-content: center;
	height: 90px;
	font-size: 16px;
	padding: 0 10px;
	.row1 {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-top: 10px;
		.row1-right {
			display: flex;
			justify-content: space-around;
			align-items: center;
		}
	}
	.row2 {
		display: flex;
		align-items: center;
		margin-top: 10px;
		.row2-left {
			white-space: nowrap;
		}
		
	}
}

</style>