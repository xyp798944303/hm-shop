<template>
	<view class="pics">
		<scroll-view class="left" scroll-y>
			<view
			@click="leftClick(index, item.id)"
			:class="active===index?'active':''" 
			v-for="(item, index) in cates"
			:key="item.id">{{item.title}}</view>
		</scroll-view>
		<scroll-view class="right" scroll-y>
			<view class="item" v-for="item in secondDate" :key="item.id">
				<image :src="item.img_url" @click="preView(item.img_url)"></image>
				<text>{{item.title}}</text>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cates: [],
				active: 0,
				secondDate: []
			}
		},
		onLoad() {
			this.getPicsCate()
		},
		methods: {
			async getPicsCate () {
				const res = await this.$myRequest ({
					url: '/api/getimgcategory'
				})
				this.cates = res.data.message
			},
			async leftClick (index,id) {
				this.active = index
				const res = await this.$myRequest({
					url: '/api/getimages/'+id
				})
				this.secondDate = res.data.message
			},
			preView (current) {
				const urls = this.secondDate.map(item=>{
					return item.img_url
				})
				uni.previewImage({
					current,
					urls
				})
			}
		}
	}
</script>

<style lang="scss">
    page {
		height: 100%;
	}
	.pics {
		height: 100%;
		display: flex;
		.left {
			width: 200rpx;
			height: 100%;
			border-right: 1px solid #eee;
			view {
				height: 60px;
				line-height: 60px;
				color: #333;
				text-align: center;
				font-size: 30rpx;
				border-top: 1px solid #eee;
			}
			.active {
				background: $shop-color;
				color: #fff;
			}
		}
		.right {
			height: 100%;
			width: 520rpx;
			margin: 10rpx auto;
			.item {
				image{
				width: 520rpx;
				height: 520rpx;
				border-radius: 5px;
			}
			text {
				font-size: 30rpx;
				line-height: 60rpx;
			}
		  }
		}
	}
</style>
