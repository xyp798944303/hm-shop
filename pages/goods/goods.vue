<template>
	<view class="goods_list">
		<goods-list :goods="goods" @goodsClick="goDetail"></goods-list>
		<view class="isover" v-if="flag">----- 我是有底线的 -----</view>
	</view>
</template>

<script>
	import goodsList from '../../components/goods-list/goods-list.vue'
	export default {
		data() {
			return {
				pageindex: 1,
				goods: [],
				flag: false
			}
		},
		onLoad() {
			this.getGoods()
		},
		onReachBottom() {
			if(this.goods.length<this.pageindex*10) return this.flag = true
			this.pageindex++
			this.getGoods()
		},
		onPullDownRefresh() {
			this.pageindex = 1
			this.goods = []
			this.flag = false
			setTimeout(()=>{
				this.getGoods(()=>{
					uni.stopPullDownRefresh()
				})
			},1000)
		},
		components: {"goods-list":goodsList},
		methods: {
			async getGoods (callBack) {
				const res = await this.$myRequest({
					url: '/api/getgoods?pageindex='+this.pageindex
				})
				// console.log(res.data)
				// res.data.message[8].img_url= "http://www.hhhmail.cn/upload/201504/20/thumb_201504200242250674.jpg"
				this.goods = [...this.goods,...res.data.message]
				callBack && callBack()
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
    .goods_list {
		background: #eee;
	}
	.isover {
		width: 100%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		// background: #fff;
		font-size: 28rpx;
	}
</style>
