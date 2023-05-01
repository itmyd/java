<template>
  <div class="container">
    <!-- 页面顶部标题 -->
    <div class="page-title">{{ pageTitle }}</div>

    <!-- 只在 index 页面显示设备列表 -->
    <div v-if="activeTab === 'index'">
      <!-- 菜品列表 -->
      <div>
        <div v-for="(category, index) in categories" :key="index">
          <h2>{{ category.name }}</h2>
          <div v-for="(item, i) in category.recipe" :key="i">
            <h3>{{ item.name }}</h3>
            <p>价格：{{ item.proce }}元</p>
            <p>折扣价：{{ item.discount }}元</p>
            <p>VIP价：{{ item.vip_discount }}元</p>
            <p>类型：{{ item.type === 1 ? '甜品' : '主食' }}</p>
            <p>数量：{{ item.quantity }}</p>
            <p>已售：{{ item.sold }}</p>
          </div>
        </div>
        <div v-if="categories.length === 0">
          暂无数据
        </div>
      </div>
    </div>
	
	 <!-- 购物车列表 -->
	    <div v-if="cartList && cartList.length > 0">
	      <div v-for="(item, index) in cartList" :key="index" class="cart-item">
	        <div>菜品名称：{{ item.food_Id }}</div>
	        <div>数量：{{ item.food_Num }}</div>
	        <div>口味：{{ item.ftaste }}</div>
	      </div>
	    </div>
	    <div v-else>
	      暂无数据
	    </div>

    <!-- 底部导航栏 -->
    <div class="tabbar">
      <div class="tab-item" @click="selectTab('index')" :class="{active: activeTab === 'index'}">
        <i class="iconfont icon-home"></i>
        <div class="text">首页</div>
      </div>
      <div class="tab-item" @click="selectTab('about')" :class="{active: activeTab === 'about'}">
        <i class="iconfont icon-about"></i>
        <div class="text">购物车</div>
      </div>
      <div class="tab-item" @click="selectTab('shezhi')" :class="{active: activeTab === 'shezhi'}">
        <i class="iconfont icon-setting"></i>
        <div class="text">设置</div>
      </div>
    </div>

    <!-- 用户 ID 标签 -->
    <div class="user-id" v-if="userId && isSettings">用户 ID: {{ userId }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeTab: "index", // 当前选中的 tab
      pageTitle: "", // 页面标题
      userId: null, // 用户 ID
      devices: [], // 设备列表
      categories: [], // 菜品列表
	  cartList: [] // 购物车列表
    };
  },
  computed: {
    // 是否为设置页面
    isSettings() {
      return this.activeTab === "shezhi";
    },
  },
  mounted() {
    // 从本地缓存中获取用户 ID
    this.userId = uni.getStorageSync("userId");

    // 获取菜品列表
    this.getList();
  },
  methods: {
    // 选中 tab
    selectTab(tab) {
      this.activeTab = tab;
      // 跳转到相应的页面
      switch (tab) {
        case "index":
          this.pageTitle = "这是主页";
          break;
        case "about":
          this.pageTitle = "购物车";
          break;
        case "shezhi":
          this.pageTitle = "这是设置";
          break;
        default:
          this.pageTitle = "";
      }
      // 跳转到对应的页面
    },
    //  菜品列表
	getList() {
	  const url = "http://116.205.181.90:2345/home/getList?uid="+this.userId; //把用户id拼接在请求后面，用于权限访问
	  fetch(url)
	    .then(response => response.json())
	    .then(data => {
	      this.categories = data;
	    })
	    .catch(error => console.error(error));
	},
	 // 获取购物车列表
    getCartList() {
      // 向后端发送带有用户 ID 的请求，获取该用户的购物车列表
      const url = "http://116.205.181.90:2345/cart/selectList?uid="+this.userId;
      fetch(url)
        .then(response => response.json())
        .then(data => {
		  console.log("接收到的数据：", data); // 在控制台输出接收到的数据
          this.cartList = data.data;
        })
        .catch(error => console.error(error));
    },
  },
};
</script>

<style>
.container {
  height: 100%;
}

.page-title {
  text-align: center;
  font-size: 18px;
  font-weight: bold;
  padding: 10px 0;
  border-bottom: 1px solid #e5e5e5;
  background-color: #fff;
}
.tabbar {
  display: flex;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 50px;
  background-color: #fff;
  justify-content: space-around;
  align-items: center;
  border-top: 1px solid #e5e5e5;
  font-size: 14px;
  color: #666;
}

.tab-item {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.active .text {
  color: #007aff;
}

.user-id {
  position: fixed;
  top: 50px;
  left: 0;
  right: 0;
  text-align: center;
  background-color: #fff;
  padding: 6px 10px;
  font-size: 12px;
  color: #666;
  border-bottom: 1px solid #e5e5e5;
}
</style>