<template>
	<view class="range">
		<a-head bgColor='#4abdb4' text="下发范围"></a-head>
		<view class="all">
			<u-checkbox-group @change="checkedAll"> 
				<u-checkbox v-model="all" active-color="#4abdb4">全部</u-checkbox>
			</u-checkbox-group>
		</view>
		<view class="part">
			<u-checkbox-group @change="checkboxGroupChange" :wrap="true">
				<u-checkbox 
					@change="checkboxChange" 
					v-model="item.checked" 
					v-for="(item, index) in list" :key="index" 
					:name="item.name"
					active-color="#4abdb4"
				>{{item.name}}</u-checkbox>
			</u-checkbox-group>
		</view>	
	</view>
</template>

<script>
export default {
	data() {
		return {
			list: [
				{
					name: '张三',
					checked: false,
					disabled: false
				},
				{
					name: '李四',
					checked: false,
					disabled: false
				},
				{
					name: '王五',
					checked: false,
					disabled: false
				}
			],
			all:false,
			valuerang:'',
		};
	},
	onLoad(options) {
		if(options){
			if(options.valuerang=='全部'){
				this.valuerang='全部';
				this.list.map(val => {
					val.checked = true;
				})
				this.all=true;
			}else{
				let s = options.valuerang.split(",");
				s.map(item=>{
					this.list.map(val=>{
						if(val.name==item){
							val.checked = true;
						}
					})			
				})
				this.all=false;
			}
		}
		
	},
	methods: {
		checkboxChange(e){
			
		},
		// 选中任一checkbox时，由checkbox-group触发
		checkboxGroupChange(e) {
			this.valuerang='';
			e.map(val=>{
				this.valuerang+=val+',';
			})
			if(e.length==this.list.length){
				this.all=true;
			}else{
				this.all=false;
			}
		},
		// 全选
		checkedAll() {
			if(this.all==true){
				this.list.map(val => {
					val.checked = true;
					this.valuerang='全部';
				})
			}else{
				this.list.map(val => {
					val.checked = false;
				})
			}
			
		}
	}
};
</script>
<style lang="less" scoped>
	.range{
		padding: 30rpx;
		background: #FFFFFF;
		min-height: 100vh;
	}
	.part{
		margin-left: 50rpx;
	}
</style>