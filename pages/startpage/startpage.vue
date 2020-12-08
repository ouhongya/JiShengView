<template>
	<view class="startPage">
		<view class="top">
			<view class="title">
				<view class="txt">
					<view>验</view>
					<view>收</view>
				</view>
				<view class="txt">
					<view>评</view>
					<view>价</view>
				</view>
			</view>

		</view>
		<view class="circle1"></view>
		<view class="circle2"></view>
		<view class="bottom">
			<image src="../../static/copyright.png" mode="" class="copyright"></image>
			<text>成都积盛电子科技有限公司版权所有</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		mounted() {
			this.tologin()
		},
		methods: {
			tologin() {	
				const {
					windowWidth,
					windowHeight
				} = uni.getSystemInfoSync();
				let ratio = windowHeight / windowWidth;
				let deadtime=0;
				if (windowWidth > 600 && ratio < 1.7) {
					//平板端30天过期
					deadtime=60 * 60 * 24 * 30
				} else {
					//手机端15天过期
					deadtime=60 * 60 * 24 * 15
				}
				if (uni.getStorageSync('hasLogin')) {
					let timeObj = uni.getStorageSync('timeObj');
					if (timeObj) {
						let Time = new Date().getTime()
						let curTime=JSON.stringify(Time)
						if ((curTime-timeObj) > deadtime) {
							uni.showToast({
								title: '登录已过期，请重新登录！',
								icon: 'none',
								duration: 1500
							})
							setTimeout(() => {
								uni.redirectTo({
									url: '/pages/login/login'
								})
							}, 1500)
						} 
					} else {
							setTimeout(() => {
								uni.redirectTo({
									url: '/pages/index/index'
								})
							}, 1500)
						}				
				} else {
					setTimeout(() => {
						uni.redirectTo({
							url: '/pages/login/login'
						})
					}, 1500)
				}
			}
		}
	}
</script>

<style lang="scss">
	.startPage {
		background: linear-gradient(180deg, #00b2a4 0%, #8cdcd6 90%);
		height: 100vh;
		width: 100%;

		.circle1 {
			width: 400rpx;
			height: 400rpx;
			background: linear-gradient(180deg, #FFFFFF 0%, #8cdcd6 100%);
			border-radius: 50%;
			opacity: 1;
			position: absolute;
			top: 100rpx;
			left: -60rpx;
		}

		.circle2 {
			width: 160rpx;
			height: 160rpx;
			background: linear-gradient(180deg, #FFFFFF 0%, #8cdcd6 100%);
			border-radius: 50%;
			opacity: 1;
			position: absolute;
			top: 460rpx;
			left: 50%;
		}

		.top {
			padding-top: 200rpx;
			width: 100%;
			position: relative;
			z-index: 99;

			.title {
				width: 180rpx;
				height: 180rpx;
				font-size: 60rpx;
				font-weight: bold;
				padding: 20rpx;
				text-align: center;
				border-radius: 20rpx;
				color: #FFFFFF;
				border: 4rpx solid #FFFFFF;
				display: flex;
				flex-direction: column;
				justify-content: space-around;
				margin: 0 auto;
				box-shadow: 0 0 12rpx #fff;

				.txt {
					margin: 10rpx 0;
					display: flex;
					align-items: center;
					justify-content: space-around;
					text-shadow: 10rpx 10rpx 10rpx rgba(0, 0, 0, 0.4);
				}
			}

		}

		.bottom {
			position: fixed;
			bottom: 20rpx;
			color: #FFFFFF;
			width: 100%;
			text-align: center;
			z-index: 99;

			text {
				font-size: 30rpx;
				height: 60rpx;
				line-height: 60rpx;
			}

			.copyright {
				width: 26rpx;
				height: 26rpx;
				margin-right: 4rpx;
			}
		}
	}
</style>
