<template>
	<view>
		<view class="search-box">
			<uni-search-bar  @input="input" :focus="true" radius="20" cancelButton="none" ></uni-search-bar>
		</view>
		
		<!-- 搜索建议列表 -->
		<view class="sugg-list" v-if="searchResult.length !== 0" >
			<view class="sugg-item" v-for="(item,index) in searchResult" :key="index" @click="gotoDetail(item)" >
				<view class="goods-name">{{item.goods_name}}</view>
				<uni-icons type="arrowright" size="16"></uni-icons>
			</view>
		</view>
		
		<!-- 历史搜索区域 -->
		<view class="history-box" v-else >
			<!-- 标题区 -->
			<view class="history-title">
				<text>搜索历史</text>
				<uni-icons type="trash" size="17" @click="clean" ></uni-icons>
			</view>
			<!-- 列表去 -->
			<view class="history-list">
				<uni-tag :text="item" inverted="true" v-for="(item,index) in histories" :key="index" @click="gotoGoodsList(item)" ></uni-tag>
			</view>
		</view>
		
	</view>
</template>

<script> 
	export default {
		data() {
			return {
				timer:null,
				kw:'',
				// 搜索结果列表
				searchResult:[],
				// 所搜关键字的历史数据
				historyList:[]
			};
		},
		
		onLoad(){
			this.historyList = JSON.parse(uni.getStorageSync('kw') || '[]')
		},
		
		methods :{
			
			// 输入事件处理函数
			input(e){
				clearTimeout(this.timer)
				this.timer = setTimeout(()=>{
					this.kw = e
					this.getSearchList()
				},500)
			},
			
			// 获取输入列表
			async getSearchList(){
				// 判断输入是否为空
				if(this.kw.trim() === '' || this.kw.length === 0) {
					this.searchResult = []
					return 
				}
				
				const {data:res} =  await uni.$http.get('/api/public/v1/goods/qsearch' , {query:this.kw})
				console.log(res)
				if(res.meta.status !== 200 ) return uni.$showMsg()
				this.searchResult = res.message
				this.saveSearchHistory()
			},
			
			// 跳转至商品详情页
			gotoDetail(item){
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id
				})
			},
			
			// 保存搜素历史记录关键词
			saveSearchHistory(){
				const set = new Set(this.historyList)
				// 先移除之前的元素 在添加新的进去 
				// 最后通过计算属性翻转数组
				set.delete(this.kw)
				set.add(this.kw)
				// 将set对象转换为数组
				this.historyList = Array.from(set)
				
				// 对搜索历史记录 进行数据持久化
				uni.setStorageSync('kw',JSON.stringify(this.historyList))
				
			},
			
			// 清除历史记录
			clean(){
				this.historyList = []
				uni.getStorageSync('kw','[]')
			},
			
			// 跳转到
			gotoGoodsList(kw){
				uni.navigateTo({
					url:'/subpkg/goods_list/goods_list?query=' + kw
				})
			}
		},
		
		computed : {
			histories(){
				return [...this.historyList].reverse()
			}
		}
	}
</script>

<style lang="scss">
 .search-box {
	 position: sticky;
	 top: 0%;
	 z-index: 999;
	 background-color: #ffaa7f;
 }
 .sugg-list {
	 padding: 0 15px;
	 .sugg-item {
		 display: flex;
		 justify-content: space-between;
		 align-items: center;
		 padding: 5px 0;
		 font-size: 14px;
		 border-bottom: 1px solid #ddd;
		 
		 .goods-name {
		 	 white-space: nowrap;
		 	 overflow: hidden;
		 	 text-overflow: ellipsis;
		 	 margin-right: 3px;
		 }
	 }
 }
 
 .history-box {
	 padding: 0 15px;
	 .history-title {
		 display: flex;
		 justify-content: space-between;
		 text-align: center;
		 border-bottom: 1px solid #ccc;
		 padding-bottom: 5px;
		 margin: 15px 0;
	 }
	 .history-list {
		 display: flex;
		 flex-wrap: wrap;
		 uni-tag {
			 margin: 5px 6px;
		 }
				
	 }
 }
 
</style>
