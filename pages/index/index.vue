<template>
	<view class="home">
		<swiper indicator-dots circular autoplay>
			<swiper-item v-for="item in swipers" :key="item.id">
				<image :src="item.img"></image>
			</swiper-item>
		</swiper>
		<view class="nav">
			<view class="nav_item" v-for="(item, index) in navs" :key="index"@click="navClick(item.path)">
				<view :class="item.icon"></view>
				<text>{{item.title}}</text>
			</view>
		</view>
		<view class="hot_goods">
			<view class="tit">推荐商品</view>
			<goods-list :goods="goods" @goodsClick="goDetail"></goods-list>
		</view>
	</view>
</template>

<script>
	import goodsList from '../../components/goods-list/goods-list.vue'
	export default {
		data() {
			return {
				swipers: [],
				goods: [],
				navs: [
					{
						icon: 'iconfont icon-ziyuan',
						title: '黑马超市',
						path: '/pages/goods/goods'
					},
					{
						icon: 'iconfont icon-guanyuwomen',
						title: '联系我们',
						path: '/pages/contact/contact'
					},
					{
						icon: 'iconfont icon-tupian',
						title: '社区图片',
						path: '/pages/pics/pics'
					},
					{
						icon: 'iconfont icon-shipin',
						title: '学习视频',
						path: '/pages/videos/videos'
					}
				]
			}
		},
		onLoad() {
            this.getSwipers()
			this.getGoods()
		},
		components: {"goods-list":goodsList},
		methods: {
			// 获取轮播图数据
			async getSwipers () {
				const res = await this.$myRequest({
					url: '/api/getlunbo'
				})
				// console.log(res)
				res.data.message[0].img = "http://m.itheima.com/images/slidead/mobile/20191213180241750x410.jpg"
				res.data.message[1].img = "http://m.itheima.com/images/slidead/mobile/20191210154717750-410.jpg"
				this.swipers = res.data.message
			},
			async getGoods () {
				const res = await this.$myRequest({
					url: '/api/getgoods?pageindex=1'
				})
				// console.log(res.data)
				res.data.message[8].img_url= "http://www.hhhmail.cn/upload/201504/20/thumb_201504200242250674.jpg"
				this.goods = res.data.message
			},
			navClick (url) {
				uni.navigateTo({
					url
				})
			},
			goDetail (id) {
				uni.navigateTo({
					url: '../goods-detail/goods-detail?id='+id
				})
			}
		}
	}
</script>

<style lang="scss">
	.home {
		swiper {
			width: 750rpx;
			height: 380rpx;
			image {
				height: 100%;
				width: 100%;
			}
		}
		.nav {
			display: flex;
			.nav_item {
				width: 25%;
				text-align: center;
				view {
					width: 120rpx;
					height: 120rpx;
					background: $shop-color;
					border-radius: 60rpx;
					margin: 10px auto;
					line-height: 120rpx;
					color: #fff;
					font-size: 50rpx;
				}
				.icon-tupian {
					font-size: 45rpx;
				}
				text {
					font-size: 30rpx;
				}
			}
		}
			.hot_goods {
				background: #eee;
				overflow: hidden;
				margin-top: 10px;
				.tit {
					height: 50px;
					line-height: 50px;
					color: $shop-color;
					text-align: center;
					letter-spacing: 20px;
					background: #fff;
					margin: 7rpx 0;
				}
			}
		}
</style>
