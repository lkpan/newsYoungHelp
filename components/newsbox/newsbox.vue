<template>
	<view class="">
		<view class="newbox">
			<view class="nodata" v-if="!item.author">
				<image src="../../static/images/nodata.png" mode=""></image>
			</view>
			<view class="pic">
				<image :src="item.picurl" mode="" class="img"></image>
		
			</view>
			<view class="content" v-if="!item.looktime">
				<view class="title">
					{{item.title}}
				</view>
				<view class="bottom">
					<view class="publisher">
						{{item.author}}
					</view>
					<view class="sufer">
						{{item.hits}}浏览量
					</view>
				</view>
			</view>
			<view class="content" v-else>
				<view class="title">
					{{item.title}}
				</view>
				<view class="bottom">
					<view class="publisher">
						{{item.author}}
					</view>
					<view class="sufer">
						{{item.looktime}}
					</view>
				</view>
			</view>
		
		</view>
	</view>
</template>

<script>
	export default {
		name: "newsbox",
		data() {
			return {

			};
		},
		props: {
			item: {
				type: Object,
				default () {
					return {
						title: '击败日韩老巢击败日韩老巢击败日韩老巢击败日韩老巢击败日韩老巢击败日韩老巢击败日韩老巢',
						author: '',
						hits: 688,
						picurl: '../../static/images/0.jpg',
						looktime: '2023-03-21'
					}
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
	.newbox {
		display: flex;

		.pic {
			.img {
				z-index: 0;
				height: 100%;
				width: 200rpx;
			}
		}

		.content {
			display: flex;
			flex-direction: column;
			justify-content: space-between;

			.title {
				padding: 10rpx 20rpx;
				display: -webkit-box;
				-webkit-box-orient: vertical;
				-webkit-line-clamp: 2;
				overflow: hidden;
				width: 250px;
				font-size: 30px;

			}

			.bottom {
				display: flex;
				color: rgb(150, 150, 150);
				font-size: 15rpx;

				.publisher {
					padding: 0 20rpx;
				}

			}
		}
	}
</style>
