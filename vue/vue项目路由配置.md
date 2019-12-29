~~~
import Vue from 'vue';//导入vue包
import App from './App';//导入App组件
import VueRouter from 'vue-router';//1.导入vue-router包
//2.手动安装VueRouter
Vue.use(VueRouter);

import goods from './components/goods/goods.vue';//导入goods组件

//3.创建路由对象
var router = new VueRouter({
  routes: [{path: '/goods', component: goods}]
});
/* eslint-disable no-new */
var vm = new Vue({
  el: '#app',
  router,//4.将路由对象挂载到vm上
  render: h => h(App)//render会把el指定的容器都清空
});

~~~

