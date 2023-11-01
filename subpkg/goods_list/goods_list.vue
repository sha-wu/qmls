<template>
	<view>
		<view class="goods-list">
			<view v-for="(item,index) in goodsList" :key="index" @click="gotoDetail(item)" >
				<my-goods :item="item" ></my-goods>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 请求参数对象
				queryObj:{
					query:'',
					cid:'',
					pagenum:1,
					pagesize:10
				},
				// 商品列表数据
				goodsList:[],
				// 页面总数
				total:0,
				// 是否正在请求数据
				isloading:false
			};
		},
		
		
		onLoad(options) {
			this.queryObj.query = options.query || ''
			this.queryObj.cid = options.cid || ''
			this.getGoodsList()
		},
		
		methods:{
			// 获取商品列表
			async getGoodsList(cb){
				this.isloading = true
				const {data: res } =  await uni.$http.get('/api/public/v1/goods/search',this.queryObj)
				if(res.meta.status !== 200 ) return uni.$showMsg()
				this.isloading = false
				cb && cb()
				this.goodsList = [ ...this.goodsList, ...res.message.goods]
				this.total = res.message.total
			},
			
			
			// 跳转至详情页
			gotoDetail(item){
						 uni.navigateTo({
						 	url:'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id
						 })
			}
			
			
			
		},
		
		// 上拉加载
		onReachBottom() {
			
			if(this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('数据加载完毕!')
			
			
			if(this.isloading) return
			// 让页码值自增加1
			this.queryObj.pagenum++
			this.getGoodsList()
		},
		
		// 下拉刷新
		 onPullDownRefresh() {
		 	// 重置关键数据
			this.queryObj.pagenum = 1
			this.total = 0
			this.isloading = false
			this.goodsList = []
			// 重新发起数据请求
			this.getGoodsList(()=>{
				uni.stopPullDownRefresh()
			})
		 }
		 
		 
		
	}
</script>

<style lang="scss">

</style>
