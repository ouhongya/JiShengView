<template>
	<view class="problemList">
		<a-head :text="headTitle" bgColor='#4abdb4'></a-head>
		<view class="scorll-content">
			<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll" scroll-left="120">
				<view class="top">
					<view class="scorll-th">
						<view class="srorll-title">
							项目序号
						</view>
					</view>
					<view class="scorll-th">
						<view class="srorll-title">
							状态
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="srorll-title">
							检查单位
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="srorll-title">
							评判项目
						</view>
					</view>
					<view class="scorll-th">
						<view class="srorll-title">
							标准
						</view>
					</view>
					<view class="scorll-th">
						<view class="srorll-title">
							整改人员
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="srorll-title">
							问题信息
						</view>
					</view>
					<view class="scorll-th">
						<view class="srorll-title">
							扣分
						</view>
					</view>
					<view class="scorll-th pic">
						<view class="srorll-title">
							问题图片
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="srorll-title">
							问题备注
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="srorll-title">
							整改措施
						</view>
					</view>
					<view class="scorll-th pic">
						<view class="srorll-title">
							整改图片
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="srorll-title">
							整改备注
						</view>
					</view>
					<view class="scorll-th">
						<view class="srorll-title">
							整改时间
						</view>
					</view>
				</view>
				<view v-for="(i,index1) in tableList" class="table-bottom">
					<view class="scorll-th">
						<view class="scorll-td">
							<view class="content">
								{{i.serial}}
							</view>
						</view>
					</view>
					<view class="scorll-th">
						<view class="scorll-td">
							<view class="content">
								待审核
							</view>
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="scorll-td detail">
							<view class="content">
								{{i.unitName}}
							</view>
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="scorll-td">
							<view class="content">
								{{i.item}}
							</view>
						</view>
					</view>
					<view class="scorll-th">
						<view class="scorll-td">
							<view class="content">
								{{i.normName}}
							</view>
						</view>
					</view>
					<view class="scorll-th">
						<view class="scorll-td">
							<view class="content">
								{{i.issueUserName}}
							</view>
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="scorll-td">
							<view class="content">
								{{i.issueContent}}
							</view>
						</view>
					</view>
					<view class="scorll-th">
						<view class="scorll-td">
							<view class="content">
								{{i.score}}
							</view>
						</view>
					</view>
					<view class="scorll-th pic">
						<view class="scorll-td">
							<view class="content">
								<!-- <view class="tips" @click="managePic1(index,index1,i.list1)" v-if="i.showPic1">管理图片</view>
										<view class="tips" @click="finishPic1(index,index1,i.fileList1)" v-else>管理完成</view>
										<view class="imgpart" v-if="i.showPic1">
											<u-swiper :list="i.list1" @click="previewImage(i.list1)"></u-swiper>
										</view>
										<view class="upload" v-else>
											<u-upload :action="action" :file-list="i.fileList1" @on-remove="onRemove" :show-progress="false"
											 :max-count="9"></u-upload>
										</view> -->
								<view class="imgpart">
									<u-swiper :list="i.list1" @click="previewImage(i.list1)"></u-swiper>
								</view>
							</view>
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="scorll-td">
							<view class="content">
								{{i.remark}}
							</view>
						</view>
					</view>
					<view class="scorll-th detail">

						<view class="scorll-td">
							<view class="content">
								
							</view>
						</view>
					</view>
					<view class="scorll-th pic">
						<view class="scorll-td">
							<view class="content">
								<view class="imgpart">
									<u-swiper :list="i.list" @click="previewImage(i.list)"></u-swiper>
								</view>
								<!-- 	<view class="tips" @click="managePic(index,index1,i.list)" v-if="i.showPic">管理图片</view>
										<view class="tips" @click="finishPic(index,index1,i.fileList)" v-else>管理完成</view>
										<view class="imgpart" v-if="i.showPic">
											<u-swiper :list="i.list" @click="previewImage(i.list)"></u-swiper>
										</view>
										<view class="upload" v-else>
											<u-upload :action="action" :file-list="i.fileList" @on-remove="onRemove" :show-progress="true"
											 :max-count="9"></u-upload>
										</view> -->
							</view>
						</view>
					</view>
					<view class="scorll-th detail">
						<view class="scorll-td">
							<view class="content">
								
							</view>
						</view>
					</view>
					<view class="scorll-th">
						<view class="scorll-td">
							<view class="content">
								<!-- {{$u.timeFormat(i.operatingTime, 'yyyy-mm-dd hh:MM:ss')}} -->
							</view>
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
		<!--页面加载动画-->
		<rfLoading :active="loading"></rfLoading>
	</view>
</template>

<script>
	import helper from '@/common/helper.js'
	export default {
		data() {
			return {
				loading: false,
				headTitle: '问题明细',
				action: 'http://192.168.1.204:8888/api/v1/uploadImage',
				tableList: [{
					//问题图片
					showPic1: true,
					list1: [{
						image: 'https://cdn.uviewui.com/uview/swiper/1.jpg',

					}],
					fileList1: [],
					//整改图片
					showPic: true,
					list: [{
							image: 'https://cdn.uviewui.com/uview/swiper/1.jpg',

						},
						{
							image: 'https://cdn.uviewui.com/uview/swiper/2.jpg',

						},
						{
							image: 'https://cdn.uviewui.com/uview/swiper/3.jpg',
						}
					],
					fileList: []
				}],
				data:[],
				id:'',
				group_id:''
			}
		},
		onLoad(options) {
			if (options.param) {
				let param = JSON.parse(options.param)
				console.log(param,'param2222')
				this.id=param.id;
				this.group_id=param.group_id;
				this.headTitle =param.name+ '问题列表';
				// let arr = []
				// arr.push(param.id)
				// this.questionsTaskList(null,arr, 0, null, null, [])
				
			} else {
				this.headTitle = '问题列表';
			}

		},
		created() {
			this.loading = true
			this.questionsTaskList(this.id, this.group_id)
		},
		mounted() {
			setTimeout(() => {
				this.loading = false;
			}, 500)
		},
		methods: {
			//问题管理图片
			managePic1(index, list) {
				this.tableList[index].showPic1 = false;
				this.tableList[index].fileList1 = list;
			},
			//问题管理完成
			finishPic1(index, fileList) {

				this.tableList[index1].showPic1 = true;
				this.tableList[index].list1 = fileList;
			},
			//整改管理图片
			managePic(index, list) {
				this.tableList[index1].showPic = false;
				this.tableList[index].fileList = list;
			},
			//整改管理完成
			finishPic(index, fileList) {
				this.tableList[index].showPic = true;
				this.tableList[index].list = fileList;
			},
			scroll(e) {

			},
			//移除图片
			onRemove(index, lists) {
				console.log(index, 'index')
				console.log(lists, 'lists')
				console.log('图片已被移除')
			},
			//预览图片
			previewImage(fileList) {
				let list = [];
				fileList.map(i => {
					list.push(i.image)
				})
				let current = list[0];
				uni.previewImage({
					current: current, // 当前显示图片的http链接  
					urls: list // 需要预览的图片http链接列表  
				})
			},
			// 上拉加载
			onreachBottom(e) {
				if (this.pageNum >= this.pages) {
					this.status = 'nomore';
					return;
				}
			},
			loadmore() {
				if (this.pageNum >= this.pages) {
					this.status = 'nomore';
					return;
				}
			},
			//拉去数据
			questionsTaskList(taskId,userId) {
				console.log('111')
				let obj = {
					uid: uni.getStorageInfoSync("USER_ID"),
					taskId:taskId,
					userId:userId
				}
				helper.request(helper.websiteUrl + 'v1/questionsTaskOne', obj, 'POST', true, res => {
					if (res.length&&res) {				
						this.pages = parseInt(res[0].total)
						let array = []
						let arr = []
						res.forEach(item => {
							
							item.issueList.forEach(key => {
								let img1 = []
								let img2 = []
								key.issueImage.forEach(val => {
									img1.push({
										title: val.name,
										image: helper.addBaseUrl(val.url),
										id: val.id
									})
									img2.push({
										title: val.name,
										url: helper.addBaseUrl(val.url),
										id: val.id
									})
								})
								arr.push({
									serial: key.serial,
									status: key.STATUS,
									unitName: key.unitName,
									item: key.item,
									issueContent: key.issueContent,
									normName: key.normName,
									issueUserName: key.issueUserName,
									rectifyName: key.rectifyName,
									score: key.score,
									issueImage: key.issueImage,
									remark: key.remark,
									rectifyImage: key.rectifyImage,
									rectifyRemark: key.rectifyRemark,
									operatingTime: key.operatingTime,
									//问题图片
									showPic1: true,
									list1: img1,
									fileList1: img2,
									//整改图片
									showPic: true,
									list: [],
									fileList: []
								})
							})
							let norm = ''
							item.normName.forEach(key => {
								norm += key.name + "    "
							})
							arr.sort((a, b) => {
								return parseInt(a.serial) - parseInt(b.serial)
							})
							array.push({
								isShow: false,
								normName: norm.substring(0, norm.length - 1),
								name: item.NAME,
								status: item.STATUS,
								unitName: item.unitName,
								total_issue: item.total_issue,
								tableList: arr
							})
			
						})
						this.data = array
						console.log(this.data,'thia.data')
						this.tableList=arr;
						this.headTitle = this.data[0].name + ' 问题列表';
					}
				})
			},
			}
			}
			
			
	
</script>

<style lang="less" scoped>
	.scorll-content {
		padding: 0 0 100rpx 0;

		.scroll-view_H {
			white-space: nowrap;
			width: 100%;
			.table-bottom{
				height: 300rpx;
			}

			.scorll-th {
				width: 200rpx;
				display: inline-block;

				.srorll-title {
					height: 60rpx;
					line-height: 60rpx;
					text-align: center;
					font-size: 28rpx;
					border: 1px solid #f6f6f6;
					background: #6ecac3;
					color: #FFFFFF;
					padding: 10rpx 0;
				}

				.scorll-td {
					width: 100%;
					border: 1px solid #f6f6f6;
					font-size: 28rpx;
					color: #606266;
					height: 300rpx;
					white-space: normal;
					overflow-y: auto;
					background: #FFFFFF;

					.content {
						padding: 10rpx 20rpx;
						line-height: 40rpx;
						white-space: normal;

						.tips {
							margin-bottom: 10rpx;
							font-size: 24rpx;
							color: #FFFFFF;
							background: #6ecac3;
							height: 60rpx;
							line-height: 60rpx;
							text-align: center;
							padding: 0 10rpx;
						}
					}
				}
			}

			.detail {
				width: 300rpx;
			}

			.pic {
				width: 500rpx;

				/deep/uni-swiper {
					height: 200rpx !important;
				}
			}
		}
	}
</style>
