<template>
	<view class="register">
		<div class="register-form">
			<form @click="formSubmit">
			<div>
						<label for="username" class="cyh">用户名:</label>
						<div>
						<input type="text" class="form-control" v-model="username" placeholder="请输入用户名"/>  
					  </div>
			</div>
			<div>
			  <label for="password" class="cyh">密码:</label>
					<div>
						<input type="password" class="form-control" v-model="password" placeholder="请输入密码"/>
					</div>
			</div>
			<div>
			  <label for="password" class="cyh">重复密码:</label>
					<div>
						<input type="password" class="form-control" v-model="confirmPassword" placeholder="请再次输入密码"/>
					</div>
			</div>
			<div>
			  <label for="password" class="cyh">电话号码:</label>
					<div>
						<input type="text" class="form-control" v-model="number" placeholder="请输入手机号码"/>
					</div>
			</div>
			<button class=" btn-primary cyh1" type="submit">注册</button>
		</form>
		<navigator class="register-login-link" url="/pages/login/login">返回登录←</navigator>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				username: "",
				password: "",
				confirmPassword: "", // 新增变量
				number: ""
			};
		},
		methods: {
			formSubmit() {
				// 校验密码是否相同
				if (this.password !== this.confirmPassword) {
					
					return;
				}

				const credentials = {
					username: this.username,
					password: this.password,
					number: this.number
				};
				uni.request({
					url: "http://116.205.181.90:2345/user/register",
					method: "POST",
					header: {
						"Content-Type": "application/x-www-form-urlencoded",
					},
					data: credentials,
					success: (response) => {
						console.log(response);
						if (response.data.code === 200) {
							uni.navigateTo({
								url: "../login/login",
							});
							uni.showToast({
								title: "注册成功",
								duration: 1000, // 设置显示时间为 1 秒钟
							});
						} 
					},
					fail: (error) => {
						console.error(error);
						uni.showToast({ 
							title: "注册失败，请稍后重试",
						});
					},
				});
			},
		},
	};
</script>

<style scoped>
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
  width: 580rpx;
  height: 75rpx;
  margin: 40rpx;
  margin-left:auto;
  margin-right: auto;
  padding: 10px;
  font-size: 16px;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  border: 1px solid #ced4da;
  border-radius: 4px;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.btn-primary:hover {
  background-color: #36a474;
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
	width: 350rpx ;
    height: 120rpx;
    border-radius: 120rpx;
    font-size: 50rpx;
    margin-top: 50rpx;
    background-color:blue;
    color:white;
}
.cyh1:hover{
    color:rgb(255, 255, 255) ;
}
page{
    height: 100%;
    background-color: rgba(0 ,191, 255,0.692);
}
</style>