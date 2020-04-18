<template>
	<view class="news">
		<view class="news_item" v-for="item in newsList" :key="item.id" @click="goDetail(item.id)">
			<image :src="item.img_url"></image>
			<view class="right">
				<view class="tit">
					{{item.title}}
				</view>
				<view class="info">
					<text>发表时间：{{item.add_time |formatData}}</text>
					<text>浏览：{{item.click}}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				newsList: []
			}
		},
		onLoad() {
			this.getNews()
		},
		methods: {
			async getNews () {
				const res = await this.$myRequest({
					url: '/api/getnewslist'
				})
				this.newsList = res.data.message
			},
			getDetail (id) {
				uni.navigateTo({
					url: '../news-detail/news-detail?id='+id
				})
			},
			goDetail (id) {
				this.getDetail(id)
			}
		}
	}
</script>

<style lang="scss">
    .news {
		.news_item {
			display: flex;
			padding: 10rpx 20rpx;
			border-bottom: 1px solid $shop-color;
			image {
				min-width: 200rpx;
				max-width: 200rpx;
				height: 150rpx;
			}
			.right {
				display: flex;
				flex-direction: column;
				justify-content: space-between;
				margin-left: 15rpx;
				.tit {
					font-size: 30rpx;
				}
				.info {
					font-size: 24rpx;
					text:nth-child(2) {
						margin-left: 30rpx;
					}
				}
			}
		}
	}
</style>
