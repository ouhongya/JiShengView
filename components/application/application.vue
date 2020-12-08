<template>
	<view class="application">
		<u-sticky :offset-top="offset">
			<view class="applist" @click="jump()">
				<view class="imgPart">
					<image src="../../static/whitepublic.png" mode=""></image>
				</view>
				<view class="title">
					公告
				</view>
			</view>
		</u-sticky>
	</view>
</template>

<script>
	export default {
		props: {
			offset: {
				type: Number,
				default: 0
			},
		},
		data() {
			return {

			}
		},
		methods: {
			jump() {
				uni.navigateTo({
					url: '../../pages/public/public'
				})
			}
		}
	}
</script>

<style lang="less" scoped>
	.applist {
		padding: 60rpx 100rpx;
		width: 100rpx;

		.imgPart {
			width: 100rpx;
			height: 100rpx;
			border-radius: 50rpx;
			background: #00b2a4;
			text-align: center;
			box-shadow: 0 0 10rpx #FFFFFF;

			image {
				width: 60rpx;
				height: 60rpx;
				margin-top: 20rpx;
			}
		}

		.title {
			color: #333333;
			font-size: 28rpx;
			margin-top: 10rpx;
			text-align: center;
		}
	}
</style>
