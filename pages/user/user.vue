<template>
	<view class="content">
		<view class="history">
			<view class="myItem">
				<image src="../../static/logo.png" mode="" class="img"></image>
				<view class="word">
					浏览历史
				</view>
			</view>
		</view>
		<view class="news"  v-for="(item,index) in listArr" :key="index">
			<newsbox @click.native='goDetail(item)' :item="{title:item.title, author:'浏览时间', hits:688,picurl:item.picurl, looktime:item.posttime}"></newsbox>
		</view>
		<view class="nohis" v-if="!listArr.length">
			<image src="../../static/images/nohis.png" mode=""></image>
			<view class="">
				暂无浏览历史
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr:[],  // 历史记录
			};
		},
		onShow() {
			this.getData()
		},
		methods:{
			getData(){
				let historyArr = uni.getStorageSync('historyArr') || []
				this.listArr = historyArr
			},
			// 跳转详情页
			goDetail(item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
		}
	}
</script>

<style lang="scss" scoped>
.content{
	display: flex;
	flex-direction: column;
	.history{
		display: flex;
		justify-content: center;
		align-items: center;
		height: 250rpx;
		.myItem{
			.img{
				width: 100rpx;
				height: 100rpx;
			}
		}
	}
	.news{
		.newsItem{
		margin: 20rpx 8rpx;
			
		}
		
	}
	.nohis{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		image:{
			font-size: 26rpx;
			color: #888;
		}
	}
}
</style>
