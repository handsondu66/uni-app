<template>
	<view>
		<view class="search-box">
			<uni-search-bar @confirm="search" @input="input" :radius="18" :cancelButton="none"></uni-search-bar>
		</view>
		
		
		<view class="sugg-list" v-if="searchResults.length !== 0">
			<view class="sugg-item" v-for="(item, i) in searchResults" :key="i" @click="gotoDetail(item)">
				<view class="goods-name"> {{item.goods_name}}</view>
				<uni-icons type="right" size="16"></uni-icons>

			</view>
		</view>
	
	
	    <view class="history-box" v-else>
			
			<view class="history-title">
				<text>搜索历史</text>
				<uni-icons type="trash" size="30"  @click="clean"></uni-icons>

			</view>
			<view class="history-list">
				<uni-tag :text="item" v-for="(item, i) in histories" :key="i" @click="gotoGoodsList(item)"> </uni-tag>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				timer: null,
				kw: '',
				searchResults: [],
				historyList:['aa', 'app']
			};
		},
		computed:{
			histories(){
				return [...this.historyList].reverse()
			}
		},
		onLoad() {
			this.historyList = JSON.parse(uni.getStorageSync('kw') || '[]')
		},
		
		methods:{
		
			input(e){
				clearTimeout(this.timer)
				this.timer = setTimeout(()=>{
					this.kw = e
				    this.getSearchList()
				}, 500)
				
			},
			async getSearchList(){
				if(this.kw.length === 0){
					this.searchResults = []
					return
				}
				
				const {data:res} = await  uni.$http.get('/api/public/v1/goods/qsearch', {query : this.kw})
				if(res.meta.status !== 200) uni.$showMsg()
				
				this.searchResults = res.message
				
				this.saveSearchHistory()
				
			},
			gotoDetail(item){
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id
				})
			},
			saveSearchHistory(){
				// this.historyList.push(this.kw)
				const set = new Set(this.historyList)
				set.delete(this.kw)
				set.add(this.kw)
				this.historyList = Array.from(set)
				
				uni.setStorageSync('kw', JSON.stringify(this.historyList))
			},
			clean(){
				this.historyList = []
				uni.setStorageSync('kw', '[]')
			},
			gotoGoodsList(kw){
				uni.navigateTo({
					url:'/subpkg/goods_list/goods_list?query=' + kw
				})
			}
			
		}
	}
</script>

<style lang="scss">
.search-box{
	position: sticky;
	top: 0;
	z-index: 999;
	
}

.sugg-list{
	padding: 0 5px;
	.sugg-item{
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 13px 0;
		font-size: 12px;
		.goods-name{
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			
		}
	}
}
.history-box{
	
	padding: 0 5px;
	.history-title{
		display: flex;
		justify-content: space-between;
		align-items: center;
		font-size: 13px;
		border-bottom: 1px solid #efefef;
	}
	.history-list{
		
	}
	
}
</style>
