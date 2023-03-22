<template>
	<view class="detail">
		<view class="title">
			{{detail.title}}
		</view>
		<view class="time">
			<view class="edit">
				作者：{{detail.author}}
			</view>
			<view class="pushTime">
				日期：{{formatTime}}
			</view>
		</view>
		<view class="content">
			<view class="word">
				<rich-text :nodes="detail.content"></rich-text>
			</view>
		</view>
		<view class="say">
			申明：请求腾讯绿石喊，内容均来源于网站有怪莫怪内容均来源于网站有怪莫怪内容均来源于网站有怪莫怪内容均来源于网站有怪莫怪
		</view>
	</view>
</template>

<script>
	import timestampToTime from '@/utils/formatTime.js'
	export default {
		data() {
			return {
				option: {}, // 传递过来的数据
				detail:{}, // 详细数据
				
			};
		},
		onLoad(e) {
			this.option = e
			this.getDetail()
		},
		methods:{
			getDetail(){
				uni.request({
					url:`https://ku.qingnian8.com/dataApi/news/detail.php`,
					data:this.option||{},
					success:res=>{
						if(res.statusCode==200){
							res.data.content = res.data.content.replace(/<img/gi,'<img style="max-width:100%"')
							this.detail = res.data
							
							// 保存结果到本地
							this.saveHistory()
						}
					}
				})
			},
			saveHistory(){
				let historyArr = uni.getStorageSync('historyArr')||[]
				let item = {
					id : this.detail.id,
					classid : this.detail.classid,
					picurl : this.detail.picurl,
					posttime : timestampToTime(Date.now()),
					title:this.detail.title,
				}
				// 存入之前进行去重
				let idx = historyArr.findIndex(i=>i.id==this.detail.id)
				if(idx>=0){
					historyArr.splice(idx,1)
				}
				historyArr.unshift(item)
				uni.setStorageSync('historyArr',historyArr)
			}
		},
		computed:{
			formatTime(){
				return timestampToTime(this.detail.posttime||0)
			}
		}
	}
</script>

<style lang="scss">
.detail{
	padding: 40rpx 20rpx;
	.content{
	}
	.title{
		font-size: 50rpx;
	}
	.time{
		display: flex;
		justify-content: space-between;
		align-items: center;
		background-color: rgb(200,200,200);
		color: black;
		padding: 0 18rpx;
		margin: 40rpx 0;
		height: 60rpx;
		font-size: 15rpx;
	}
	.say{
		margin-top: 50rpx;
		background-color: lightpink;
		font-size: 25rpx;
		line-height: 40rpx;
	}
}
</style>
