<template>
	<view class="my-search-container" :style="{ backgroundColor: bgcolor }" >
		<view class="my-search-box" :style="{ borderRadius: radius + 'px' }" @click="searchBoxHandler()" >
			<!-- 使用uni-ui的图标组件 -->
			<uni-icons type="search" size="17"></uni-icons>
			<text class="placeholder">搜索</text>
		</view>
	</view>
</template>

<script>
	export default {
		name:"my-search",
		data() {
			return {
				
				
			};
		},
		props: {
			bgcolor : {
				type: String,
				default: '#ffaa7f'
			},
			radius: {
				type: Number,
				default: 18 // px
			}
		},
		methods:{
			searchBoxHandler(){
				console.log( '~~~' )
				this.$emit('click')
			}
		}
		
	}
</script>

<style lang="scss">
 .my-search-container {
	 display: flex;
	 align-items: center;
	 padding: 0 10px;
	 height: 50px;
	 // background-color: #ffaa7f;
	 .my-search-box {
		 display: flex;
		 justify-content: center;
		 align-items: center;
		 width: 100%;
		 height: 30px;
		 background-color: #fff;
		 // border-radius: 18px;
		 
		 .placeholder {
			 font-size: 15PX;
			 margin-left: 5px;
		 }
	 }
 }
</style>