<template>
	<view class="my-search-container" :style="{'background-color': bgcolor}" @click="searchBoxHandler">
		<view class="my-search-box" :style="{'border-radius': raduis+'px'}">
			<uni-icons type="search" size="20"></uni-icons>
			<text>搜索</text>			
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
		props:{
			bgcolor:{
				type:String,
				default: '#C00000'
			},
			raduis:{
				type: Number,
				default: 18
			}
		},
		methods:{
			searchBoxHandler(){
				console.log("-----")
				this.$emit('click')
			}
		}
	}
</script>

<style lang="scss">
	
.my-search-container{
	height: 50px;

	display: flex;
	align-items: center;
	padding: 0, 10px;
	.my-search-box{
		height: 30px;
		background-color: #FFFFFF;
	
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		text{
			font-size: 15px;
			margin-left: 5px;
		}
	}
}
</style>