<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')					
				const {
					windowWidth,
					windowHeight,
					pixelRatio					
				} = uni.getSystemInfoSync();
				let ratio=windowHeight/windowWidth;
				if(windowWidth>600&&ratio<1.7&&windowHeight>600){
					// #ifdef APP-PLUS
					plus.screen.lockOrientation('landscape-primary');
					// #endif
				}else{
					// #ifdef APP-PLUS
					plus.screen.lockOrientation('portrait-primary'); //锁死屏幕方向为竖屏
					// #endif
				}
		},
		onHide: function() {
			console.log('App Hide')
		},
		methods: {
			getPhoneHeight() {
				let that = this;
				var Height = '1';
				uni.getSystemInfo({
					success: function(res) {
						Height = res.statusBarHeight
		
					}
				})
				return Height
			}		
		}
	}
</script>

<style lang="less">
	page{
		background: #F8F8F8;
		min-height: 100vh;
	}
	/*每个页面公共css */
	// 时间展示
	/deep/.u-calendar__action {
		display: flex;
		justify-content: center;
		align-items: center;
	}
	/deep/.u-input {
		align-items: center;
	}
	/deep/.u-td{
		word-wrap: break-word;
		 word-break: break-all;
	}
	// 空状态
	.empty{
		width: 100%;
		text-align: center;
		margin: 150rpx 0 60rpx;
		image{
			width: 180rpx;
			height: 120rpx;
		}
		.changing {
			width: 50rpx !important;
			height: 50rpx !important;
		}
		.empty-tips{
			color: #bfbfbf;
			margin-top: 10rpx;
			font-size: 28rpx;
		}
	}
</style>
