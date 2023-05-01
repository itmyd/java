<template>
	<view class="register">
		<div class="register-form">
			<form @click="formSubmit">
			<div>
						<label for="username" class="cyh">用户名:</label>
						<div class="cyh1">
						<input type="text" name="username" class="form-control" v-model="username" placeholder="请输入用户名"/>  
					  </div>
			</div>
			<div>
			  <label for="password" class="cyh">密码:</label>
					<div class="cyh1">
						<input type="password" name="password" class="form-control" v-model="password" placeholder="请输入密码"/>
					</div>
			</div>
			<button class=" btn-primary cyh1" type="submit">登录</button>
		</form>
		<navigator class="register-login-link" url="/pages/register/register">未注册？立即注册←</navigator>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: '',
				password: ''
			}
		},
		methods: {
			formSubmit() {
				const credentials = {
					username: this.username,
					password: this.password
				}
				uni.request({
					url: 'http://116.205.181.90:2345/user/login',
					method: 'POST',
					header: {
						'Content-Type': 'application/x-www-form-urlencoded'
					}, // 添加'Content-Type'头部

					data: credentials,
					success: (response) => {
						console.log(response); // 打印响应结果
						if (response.data.code === 200) {
							const userId = response.data.data; // 获取用户 ID
							// 将用户 ID 存储到本地缓存中，键名为 'userId'
							uni.setStorageSync('userId', userId);
							
							// 登录成功，跳转到主页
							uni.navigateTo({
								url: '../index/index'
							});
							uni.showToast({
								title: "登录成功",
								duration: 1000, // 设置显示时间为 1 秒钟
							});
						} 
					},
					fail: (error) => {
						// 处理网络故障或其他异常
						console.error(error)
						uni.showToast({
							title: '登录失败，请稍后重试'
						})
					}
				})
			}
		}
	}
</script>


<style>
.register {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  background-color: #f5f5f5;
}

.register-form {
  max-width: 450px;
  padding: 40px;
  border-radius: 8px;
  box-shadow: 0 0 12px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

.register-title {
  margin-top: 0;
  margin-bottom: 24px;
  font-size: 28px;
  font-weight: bold;
  text-align: center;
  color: #333;
}

.form-control {
  display: block;
  width: 100%;
  padding: 10px;
  font-size: 16px;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  border: 1px solid #ced4da;
  border-radius: 4px;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.register-login-link {
  margin-top: 16px;
  display: block;
  text-align: center;
  color: #666;
}

.cyh{
    font-weight: bold;
    padding: 8rpx;
}
.cyh1{
    width: 580rpx;
    height: 75rpx;
    margin: 40rpx;
    margin-left:auto;
    margin-right: auto;
    border-radius: 25rpx;
}
.cyh2{
	width: 350rpx ;
    height: 120rpx;
    border-radius: 120rpx;
    font-size: 50rpx;
    margin-top: 50rpx;
    background-color:blue;
    color:white;
}
.cyh2:hover{
    color:rgb(255, 255, 255) ;
}
page{
    height: 100%;
    background-color: rgba(0 ,191, 255,0.692);
}
.btn-primary:hover {
  background-color: #36a474;
}
</style>