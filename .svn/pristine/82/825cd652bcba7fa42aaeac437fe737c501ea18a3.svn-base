import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

App.mpType = 'app'
//ui组件
import uView from "uview-ui";
Vue.use(uView);
//头部
import Ahead from '@/components/Ahead/index.vue';
Vue.component("a-head", Ahead)
//页面加载动画
import rfLoading from '@/components/rf-loading/index.vue';
Vue.component('rfLoading', rfLoading);

const app = new Vue({
	...App
})
app.$mount()
