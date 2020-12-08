<template>
	<view class="task">
		<a-head bgColor='#4abdb4' :text="taskhead"></a-head>
		<view class="contain">
			<view class="content">
				<u-cell-group>
					<u-cell-item title="任务名称" :title-style="titleStyle" arrow-direction="right" :value="taskname" @click="taskchange"></u-cell-item>
					<u-cell-item title="检查标准" :title-style="titleStyle" arrow-direction="right" :value="standard" @click="standardchange"></u-cell-item>
				</u-cell-group>
			</view>
			<view class="content">
				<view class="title">任务信息</view>
				<u-cell-group>
					<u-cell-item title="单位名称" :title-style="titleStyle" arrow-direction="right" :value="company" @click="companychange"></u-cell-item>
					<u-cell-item title="子单位" :title-style="titleStyle" arrow-direction="right" :value="child" @click="childchange"></u-cell-item>
					<u-cell-item title="任务期限" :title-style="titleStyle" arrow-direction="right" :value="date" @click="datechange"></u-cell-item>
					<view class="flag" v-if="flag">
						<u-cell-item title="下发频次" :title-style="titleStyle" arrow-direction="right" :value="frequency" @click="frechange"></u-cell-item>
						<view class="right" @click="flagChange()">
							自定义频次
						</view>
					</view>
					<view class="flag" v-else>
						<u-cell-item title="自定义下发频次" :title-style="titleStyle" arrow-direction="right" :value="selfrequency" @click="selfrechange"></u-cell-item>
						<view class="right" @click="flagChange()">
							快捷选择频次
						</view>
					</view>
					<!-- <u-cell-item title="检查地点" :title-style="titleStyle" arrow-direction="right" :value="city" @click="citychange"></u-cell-item> -->
					<u-cell-item title="检查地址" :title-style="titleStyle" arrow-direction="right" :value="address" @click="addresschange"></u-cell-item>
					<u-cell-item title="组长" :title-style="titleStyle" arrow-direction="right" :value="leader" @click="leadchange"></u-cell-item>
				</u-cell-group>
			</view>
			<!-- 修改任务名称 -->
			<u-modal v-model="showtask" title="任务名称" cancel-text="取消" confirm-text="确定" confirm-color="#1fac9c"
			 :show-cancel-button="true" @confirm="taskconfirm">
				<view class="slot-content">
					<view class="model">
						<u-input v-model="taskpart" placeholder="请输入你的任务名称" type="textarea" :autoHeight="true" />
					</view>
				</view>
			</u-modal>
			<!-- 修改单位名称-->
			<u-select v-model="showcompany" mode="single-column" :default-value="showcompanyIndex" :list="selectcompany"
			 @confirm="companyconfirm" confirm-color="#1fac9c"></u-select>
			<!-- 修改子单位 -->
			<u-select v-model="showchild" mode="single-column" :default-value="selectchildIndex" :list="selectchild" @confirm="childconfirm"
			 confirm-color="#1fac9c"></u-select>
			<!-- 修改日期 -->
			<u-calendar v-model="showdate" mode="range" @change="dateconfirm" max-date="2030-01-01"></u-calendar>
			<!-- 修改下发频次-->
			<u-select v-model="showfre" mode="single-column" :default-value="showfreIndex" :list="selectfre" @confirm="freconfirm"
			 confirm-color="#1fac9c"></u-select>
			<!-- 自定义频次 -->
			<u-modal v-model="showselfre" title="自定义下发频次" cancel-text="取消" confirm-text="确定" confirm-color="#1fac9c"
			 :show-cancel-button="true" @confirm="selfreconfirm" ref="freModal">
				<view class="slot-content">
					<view class="model">
						<view class="number">
							<text>每</text>
							<u-input v-model="selfrepart" placeholder="请输入数字" type="number" :border="true" /><text>天一次</text>
						</view>

					</view>
				</view>
			</u-modal>
			<!-- 修改地点 -->
			<!-- <u-picker mode="region" v-model="showcity" :params="params1" confirm-color="#1fac9c" @confirm="cityconfirm"></u-picker>
			 -->
			<!-- 修改详细地址 -->
			<u-modal v-model="showaddress" title="检查地址" cancel-text="取消" confirm-text="确定" confirm-color="#1fac9c"
			 :show-cancel-button="true" @confirm="addressconfirm">
				<view class="slot-content">
					<view class="model">
						<u-input v-model="addresspart" placeholder="请输入检查地址" type="textarea" :auto-height="true" />
					</view>
				</view>
			</u-modal>
			<!-- 修改组长 -->
			<u-select v-model="selectleadFlag" mode="single-column" :default-value="selectleadIndex" :list="selectlead" @confirm="leadconfirm"
			 confirm-color="#1fac9c"></u-select>
		</view>
		<view class="action">
			<view class="bottom">
				<view class="cancel" @click="cancel">取消</view>
				<view class="save" @click="save">保存</view>
			</view>
		</view>
		<u-toast ref="uToast" />
		<!--页面加载动画-->
		<rfLoading :active="loading"></rfLoading>
	</view>
</template>

<script>
	import helper from '@/common/helper.js'
	export default {
		name: 'attention',
		data() {
			return {
				taskhead: '新建任务',
				flag: true,
				id: '',
				taskname: '',
				standard: '',
				company: '',
				child: '',
				date: '',
				frequency: '',
				selfrequency: '',
				selfrequencynum: '',
				city: '',
				address: '',
				leader: '',
				showtask: false,
				showcompany: false,
				showfre: false,
				showselfre: false,
				showdate: false,
				showcity: false,
				showaddress: false,
				showlead: false,
				showchild: false,
				selectleadFlag: false,
				taskpart: '',
				addresspart: '',
				selfrepart: '',
				titleStyle: {
					color: '#333333',
					fontSize: '30rpx',
				},
				selectstandard: [{
						value: '1',
						label: 'xxx标准1'
					},
					{
						value: '2',
						label: 'xxx标准2'
					}
				],
				selectcompany: [],
				selectchild: [],
				selectfre: [{
						value: '0',
						label: '暂无'
					},
					{
						value: '1',
						label: '每天一次'
					},
					{
						value: '7',
						label: '每周一次'
					},
					{
						value: '30',
						label: '每月一次'
					}
				],
				selectlead: [],
				params1: {
					year: true,
					month: true,
					day: true
				},
				params1: {
					province: true,
					city: true,
					area: true
				},
				uid: null,
				selectchildIndex: [0],
				showstandardIndex: [0],
				showfreIndex: [0],
				selectleadIndex: [0],
				showcompanyIndex: [0],
				moduleList: null,
				moduleids: [],
				loading: false
			}
		},
		created() {
			let _this = this;
			_this.uid = uni.getStorageSync("USER_ID")
			this.loading = true;
			_this.queryUnitAll();
			_this.queryUserBySpecialty();
			uni.$on('frequency', res => {
				this.flag = res.flag;
			})
		},
		mounted() {
			setTimeout(() => {
				this.loading = false;
			}, 500)
		},
		onLoad(options) {
			// //监听事件
			//    uni.$once('module',res=>{  
			//        this.moduleList=res.data
			// 	   this.standard = this.moduleList.name
			//     })  
			// 	//监听事件
			// 	   uni.$once('moduleids',res=>{  
			// 	       this.moduleids=res.data
			// 	    })  
			if (options.id) {
				this.queryTaskToOne(options.id)
				this.id = options.id;
				this.taskhead = '编辑任务';
			} else {
				this.id = '';
				this.taskhead = '新建任务';
			}
		},
		onShow() {
			if(uni.getStorageSync('module')){
				this.moduleList = uni.getStorageSync('module')
				this.standard = this.moduleList.name.substring(0, this.moduleList.name.length-1)
			}
			if(uni.getStorageSync('moduleids')){
				this.moduleids = uni.getStorageSync('moduleids')
			}
			
			
		},
		methods: {
			taskchange() {
				this.showtask = true;
			},
			taskconfirm() {
				this.taskname = this.taskpart;
				this.showtask = false;
			},
			standardchange() {
				// //触发事件
				// 	uni.$emit('module', {  
				// 	        data:this.moduleList
				// 	});
				// //触发事件
				// 	uni.$emit('moduleids', {  
				// 			 data: this.moduleids
				// 	  });
				uni.setStorageSync('module', this.moduleList)
				uni.setStorageSync('moduleids', this.moduleids)
				uni.navigateTo({
					url: '/pages/task/standard'
				})
			},
			companychange() {
				this.showcompany = true;
			},
			companyconfirm(e) {
				this.company = e[0].label;
				this.querySiteAll(e[0].value)
				this.showcompany = false;
				this.child = ""
			},
			childchange() {
				this.showchild = true;
			},
			childconfirm(e) {
				this.child = e[0].label;
				this.showchild = false;
			},
			datechange() {
				this.showdate = true;
			},
			dateconfirm(e) {
				this.date = e.startDate + '至' + e.endDate;
			},
			frechange() {
				this.showfre = true;
			},
			freconfirm(e) {
				this.frequency = e[0].label;
				this.showfre = false;
			},
			selfrechange() {
				this.showselfre = true;
			},
			selfreconfirm() {
				if (!/^[1-9]\d*$/.test(this.selfrepart)) {
					this.$refs.uToast.show({
						title: '请输入检查地址',
						type: 'warning'
					})
					return;
					this.$refs.freModal.clearLoading();
				} else {
					this.selfrequency = '每' + this.selfrepart + '天一次';
					this.selfrequencynum = this.selfrepart;
					this.showselfre = false;
				}
			},
			flagChange() {
				this.flag = !this.flag
			},
			citychange() {
				this.showcity = true;
			},
			cityconfirm(e) {
				this.city = e.province.label + '-' + e.city.label + '-' + e.area.label;
				this.showcity = true;
			},
			addresschange() {
				this.showaddress = true;
			},
			addressconfirm() {
				this.address = this.addresspart;
				this.showaddress = false;
			},
			leadchange() {
				this.selectleadFlag = true;
			},
			leadconfirm(e) {
				this.leader = e[0].label
				this.selectleadFlag = false;
			},
			cancel() {
				uni.navigateBack()
				this.taskname = '',
					this.standard = '',
					this.company = '',
					this.child = '',
					this.date = '',
					this.address = '',
					this.frequency = '',
					this.selfrequency = '',
					this.selfrequencynum = '',
					this.leader = '',
					this.id = '',
					this.uid = null,
					this.selectchildIndex = [0],
					this.showstandardIndex = [0],
					this.showfreIndex = [0],
					this.selectleadIndex = [0],
					this.showcompanyIndex = [0],
					this.moduleList = null
				uni.removeStorageSync('module')
				uni.removeStorageSync('moduleids')
			},
			save() {
				if (!this.taskname) {
					this.$refs.uToast.show({
						title: '请输入任务名称',
						type: 'warning'
					})
					return;
				}
				if (!this.company) {
					this.$refs.uToast.show({
						title: '请选择单位名称',
						type: 'warning'
					})
					return;
				}
				if (!this.child) {
					this.$refs.uToast.show({
						title: '请选择子单位',
						type: 'warning'
					})
					return;
				}
				if (!this.date) {
					this.$refs.uToast.show({
						title: '请选择任务期限',
						type: 'warning'
					})
					return;
				}
				if (this.flag) {
					if (!this.frequency) {
						this.$refs.uToast.show({
							title: '请选择下发频次',
							type: 'warning'
						})
						return;
					}
				} else {
					if (!this.selfrequencynum) {
						this.$refs.uToast.show({
							title: '请输入自定义下发频次',
							type: 'warning'
						})
						return;
					}
				}
				if (!this.address) {
					this.$refs.uToast.show({
						title: '请输入检查地址',
						type: 'warning'
					})
					return;
				}
				if (!this.leader) {
					this.$refs.uToast.show({
						title: '请选择组长',
						type: 'warning'
					})
					return;
				}
				if (this.id == '') {
					if (!this.standard) {
						this.$refs.uToast.show({
							title: '请选择检查标准',
							type: 'warning'
						})
						return;
					}
					let moduleList = uni.getStorageSync("module");
					let _this = this
					this.status = 'loading';
					let unitId = _this.selectcompany.find((v) => {
						return v.label == _this.company
					}).value;
					let siteId = _this.selectchild.find((v) => {
						return v.label == _this.child
					}).value;
					let starTime = _this.date.substring(0, _this.date.indexOf("至"));
					let endTime = _this.date.substring(_this.date.indexOf("至") + 1, _this.date.length);
					let groupId = _this.selectlead.find((v) => {
						return v.label == _this.leader
					}).value;
					let frequency = '';
					if (this.flag) {
						frequency = this.selectfre.find((v) => {
							return v.label == this.frequency
						}).value;

					} else {
						frequency = this.selfrequencynum;
					}
					let param = {
						uid: _this.uid,
						userId: _this.uid,
						name: _this.taskname,
						unitId: unitId,
						siteId: siteId,
						starTime: starTime,
						endTime: endTime,
						location: _this.address,
						groupId: groupId,
						frequency: frequency,
						type: 1,
						taskDetails: moduleList.userModule,
						excelId: moduleList.excel
					}
					let data = {
						uid: _this.uid,
						param: JSON.stringify(param)
					}
					uni.$off('frequency');
					helper.request(helper.websiteUrl + 'v1/createdTask', data, 'POST', true, res => {
						uni.setStorageSync("flagtask", true);
						_this.cancel();
					})
				} else {
					this.status = 'loading';
					let unitId = this.selectcompany.find((v) => {
						return v.label == this.company
					}).value;
					let siteId = this.selectchild.find((v) => {
						return v.label == this.child
					}).value;
					let starTime = this.date.substring(0, this.date.indexOf("至"));
					let endTime = this.date.substring(this.date.indexOf("至") + 1, this.date.length);
					let groupId = this.selectlead.find((v) => {
						return v.label == this.leader
					}).value;
					let frequency = '';
					if (this.flag) {
						frequency = this.selectfre.find((v) => {
							return v.label == this.frequency
						}).value;

					} else {
						frequency = this.selfrequencynum;
					}
					let param = {
						id: this.id,
						uid: this.uid,
						userId: this.uid,
						name: this.taskname,
						unitId: unitId,
						siteId: siteId,
						starTime: starTime,
						endTime: endTime,
						location: this.address,
						groupId: groupId,
						frequency: frequency,
						type: 1,
						taskDetails: null,
						excelId: null
					}
					let data = {
						uid: this.uid,
						param: JSON.stringify(param)
					}
					//自定义频次或者不是
					uni.$emit('frequency', {
						flag: this.flag
					});
					helper.request(helper.websiteUrl + 'v1/editTask', data, 'POST', true, res => {
						uni.setStorageSync("flagtask", true);
						this.cancel();
					})
				}
			},
			//获取用户选择的模块值
			async userModule() {
				await uni.getStorage({
					key: 'USER_MODULE',
					success: function(res) {
						return JSON.parse(res.data)
					}
				});
			},
			//拉去单位列表
			queryUnitAll() {
				let param = {
					uid: this.uid
				}
				helper.request(helper.websiteUrl + 'v1/queryUnitAll', param, 'POST', true, res => {
					if (res.length != 0) {
						res.forEach(item => {
							this.selectcompany.push({
								value: item.UNIT_ID,
								label: item.NAME
							})
						})
					}
				})
			},
			//拉去站点列表
			querySiteAll(id) {
				let param = {
					uid: this.uid,
					id: id
				}
				this.selectchild = []
				helper.request(helper.websiteUrl + 'v1/querySiteAll', param, 'POST', true, response => {
					if (response.length != 0) {
						response.forEach(item => {
							this.selectchild.push({
								value: item.UNIT_ID,
								label: item.NAME
							})
						})
					}
				})
			},
			//拉去组长列表
			queryUserBySpecialty() {
				if (this.id == '') {
					let param = {
						uid: this.uid
					}
					helper.request(helper.websiteUrl + 'v1/queryUserBySpecialty', param, 'POST', true, res => {
						if (res.length != 0) {
							res.group.forEach(item => {
								this.selectlead.push({
									value: item.USER_ID,
									label: item.NAME
								})
							})
						}
						let q = -1;
						this.selectlead.forEach(item => {
							q++
							if (this.uid == item.value) {
								this.selectleadIndex = [q]
								this.leader = item.label
							}
						})
					})
				}
			},
			//编辑任务回显数据
			queryTaskToOne(task_id) {
				let param = {
					uid: this.uid,
					task_id: task_id
				}
				helper.request(helper.websiteUrl + 'v1/queryTaskToOne', param, 'POST', true, res => {
					let param = {
						uid: this.uid,
						id: res.unit_id
					}
					this.selectchild = []
					helper.request(helper.websiteUrl + 'v1/querySiteAll', param, 'POST', true, response => {
						response.forEach(item => {
							this.selectchild.push({
								value: item.UNIT_ID,
								label: item.NAME
							})
						})
						this.taskname = res.taskName
						this.taskpart = res.taskName
						let str = ''
						res.normName.forEach(item => {
							str += item + "、"
						})
						this.standard = str.substring(0, str.length - 1)
						let q = -1;
						this.selectcompany.forEach(item => {
							q++;
							if (item.value == res.unit_id) {
								this.showcompanyIndex = [q]
								this.company = item.label
							}
						})
						let w = -1;
						this.selectchild.forEach(item => {
							w++;
							if (item.value == res.site_id) {
								this.selectchildIndex = [w]
								this.child = item.label
							}
						})
						this.date = this.dateFtt("yyyy-MM-dd",new Date(res.star_time))+ "至" + this.dateFtt("yyyy-MM-dd",new Date(res.end_time))
						if (res.frequency == 1 || res.frequency == 7 || res.frequency == 30) {
							this.flag = true
							let e = -1;
							this.selectfre.forEach(item => {
								e++;
								if (res.frequency == item.value) {
									this.showfreIndex = [e]
									this.frequency = item.label
								}
							})
						} else {
							this.flag = false
							this.selfrequency = '每' + res.frequency + '天一次';
							this.selfrepart = res.frequency
							this.selfrequencynum = res.frequency
						}
						this.address = res.location
						this.addresspart = res.location
						helper.request(helper.websiteUrl + 'v1/queryUserBySpecialty', param, 'POST', true, resgroup => {
							if (resgroup.length != 0) {
								resgroup.group.forEach(item => {
									this.selectlead.push({
										value: item.USER_ID,
										label: item.NAME
									})
								})
								let q = -1;

								this.selectlead.forEach(item => {
									q++
									if (item.value == res.group_id) {
										this.selectleadIndex = [q]
										this.leader = item.label
									}
								})
							}
						})
					})
				})
			},
			dateFtt(fmt,date)   { //author: meizz
			  var o = {   
			    "M+" : date.getMonth()+1,                 //月份   
			    "d+" : date.getDate(),                    //日   
			    "h+" : date.getHours(),                   //小时   
			    "m+" : date.getMinutes(),                 //分   
			    "s+" : date.getSeconds(),                 //秒   
			    "q+" : Math.floor((date.getMonth()+3)/3), //季度   
			    "S"  : date.getMilliseconds()             //毫秒   
			  };   
			  if(/(y+)/.test(fmt))   
			    fmt=fmt.replace(RegExp.$1, (date.getFullYear()+"").substr(4 - RegExp.$1.length));   
			  for(var k in o)   
			    if(new RegExp("("+ k +")").test(fmt))   
			  fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));   
			  return fmt;   
			}
		}
	};
</script>

<style lang="less" scoped>
	/deep/.u-cell__value {
		line-height: 48rpx;
	}

	.model {
		padding: 30rpx;

		.number {
			display: flex;
			align-items: center;
			margin: 0 auto;

			text {
				margin: 0 10rpx;
				color: #606266;
			}
		}
	}

	.contain {
		min-height: 100vh;
		background-color: #fafafa;
	}

	.head {
		display: flex;
		justify-content: space-between;
		height: 160rpx;
		line-height: 160rpx;
		padding: 0 30rpx;
		background: #ffffff;
		margin-bottom: 30rpx;

		.title {
			font-size: 36rpx;
			font-family: PingFang SC;
			font-weight: bold;
			line-height: 160rpx;
			color: #333333;
			opacity: 1;
			// background: #ffffff;
		}

		.img {
			display: flex;
		}
	}

	image {
		width: 120rpx;
		height: 120rpx;
		border-radius: 50%;
		margin: 30rpx 10rpx;
	}

	.content {
		background: #ffffff;
		margin-bottom: 30rpx;

		.flag {
			.right {
				font-size: 24rpx;
				color: #999999;
				padding: 0 30rpx 0 40rpx;

			}
		}

		.title {
			height: 90rpx;
			font-size: 36rpx;
			font-family: PingFang SC;
			font-weight: bold;
			line-height: 90rpx;
			color: #333333;
			opacity: 1;
			padding: 0 30rpx;

			text {
				height: 33rpx;
				font-size: 24rpx;
				font-family: PingFang SC;
				font-weight: 400;
				line-height: 33rpx;
				color: #999999;
				opacity: 1;
				margin-left: 30rpx;
			}
		}
	}

	.action {
		position: fixed;
		width: 100%;
		background: #FFFFFF;
		bottom: 0;
		z-index: 99;

		.bottom {
			display: flex;
			justify-content: space-around;
			align-items: center;
			height: 100rpx;
			line-height: 100rpx;
			text-align: center;
			color: #FFFFFF;

			.cancel {
				width: 40%;
				height: 80rpx;
				line-height: 80rpx;
				border-radius: 40rpx;
				border: 2rpx solid #dedede;
				color: #606266;
			}

			.save {
				width: 40%;
				height: 80rpx;
				line-height: 80rpx;
				background: #4abdb4;
				border-radius: 40rpx;

			}
		}

	}
</style>
