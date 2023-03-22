<template>
	<view class="home">
		<scroll-view scroll-x="true" class="navscroll">
			<view class="item" :class="index==high ? 'active' : ''" v-for="(i,index) in navData" :key='i.id' @click="clickNav(index,i.id)">{{i.classname}}
			</view>
		</scroll-view>
		<view class="content">
			<!-- 一行新闻列表 -->
			<view class="row" v-for="(item,index) in newslist" :key="item.id" @click='goDetail(index,item)'>
				<newsbox  
					:item="{title:item.title, author:item.author, hits:item.hits,picurl:item.picurl}">
				</newsbox>
			</view>
			<!-- 显示没有更多和数据加载中 -->
			<view class="nomore" v-if="newslist.length">
				<view class="">
					
				</view>
				<view class="" v-if="loading==1">数据加载中...</view>
				<view class="" v-if="loading==2">没有更多了~~~</view>
			</view>
			<view class="nodata" v-if="!newslist.length">
						<image src="../../static/images/nodata.png" mode=""></image>
					</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				high: 0, // 高亮index
				navData: [], // 导航栏数据
				newslist: [], // 新闻列表数据
				currentPage:1, // 当前新闻列表页
				loading:0, //正在加载中
			}
		},
		onLoad() {
			this.getNavData()
			this.getNewsData()
		},
		onReachBottom() {
			if(this.loading==2){
				return
			}
			this.currentPage++
			this.getNewsData()
			this.loading=1
		},
		methods: {
			// 修改高亮nav
			clickNav(i,id) {
				this.high = i
				this.currentPage = 1
				this.newslist = []
				this.loading = 0
				this.getNewsData(id)
			},
			// 跳转详情页
			goDetail(index,item){
				uni.navigateTo({
					url:`/pages/detail/detail?cid=${item.classid}&id=${item.id}`
				})
			},
			// 获取导航列表数据
			getNavData(){
				uni.request({
					url:'https://ku.qingnian8.com/dataApi/news/navlist.php',
					success:res=>{
						if(res.statusCode==200){
							this.navData = res.data
						}
					}
				})
			},
			// 获取新闻列表数据
			getNewsData(id){
				uni.request({
					url:'https://ku.qingnian8.com/dataApi/news/newslist.php',
					data:{
						num:5, // 数据条数
						cid:id?id:'', // 新闻id
						page:this.currentPage, // 当前显示新闻列表页
					},
					success:res=>{
						if(res.data.length==0){
							this.loading = 2
						}
						this.newslist = [...this.newslist,...res.data]
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.home {
		.navscroll {
			position: fixed;
			z-index: 3;
			top: var(--window-top);
			height: 100rpx;
			background: white;
			white-space: nowrap;
			background-color: #fff;

			/deep/ ::-webkit-scrollbar {
				width: 4px !important;
				height: 1px !important;
				overflow: auto !important;
				background: transparent !important;
				-webkit-appearance: auto !important;
				display: block;
			}

			.item {
				font-size: 40rpx;
				display: inline-block;
				line-height: 100rpx;
				padding: 0 30rpx;

				&.active {
					color: #31c27c
				}
			}
		}

		.content {
			padding-top: 130rpx;

			.row {
				border-bottom: 1px dotted #efefef;
				padding: 15rpx 0;
			}
		}
		.nomore{
			text-align: center;
			font-size: 26rpx;
			color: #888;
			padding: 20rpx;
		}
	}
</style>
