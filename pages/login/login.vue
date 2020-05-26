<template>
	<view class="zai-box">
		<image src="../../static/zaizai-login/login.png" mode='aspectFit' class="zai-logo"></image>
		<view class="zai-title">小白厨房</view>
		<view class="zai-form">
			<input class="zai-input" placeholder-class placeholder="请输入用户名" v-model="user"/>
			<input class="zai-input" placeholder-class password placeholder="请输入密码" v-model="password"/>
			<view class="zai-label">忘记密码？</view>
			<button class="zai-btn" @click="login">立即登录</button>
			<navigator url="../login/register" hover-class="none" class="zai-label">还没有账号？点此注册.</navigator>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				user:"",
				password:"",
			};
		},
		methods:{
			login()
			{
				var userInfo = {
					user:this.user,
					password:this.password,
				};
				console.log("user:" + this.user);
				console.log("password:" + this.password);
				uni.request({
				    url: 'https://pope.utools.club/login', //仅为示例，并非真实接口地址。
				    data: {
				        nickname:this.user,
						password:this.password
				    },
				    method:'POST',
				    header: {
				        'content-type': 'application/x-www-form-urlencoded', 
				    },
				    success: (res) => {
				        console.log(res.data);
				        this.text = 'request success';
						if(res.data.code == 1)
						{
							uni.setStorage({
								key: 'uid',
								data: res.data.data.id,
								success: function() {
									console.log('success');
								}
							});
							uni.setStorage({
								key: 'nickName',
								data: res.data.data.nickname,
								success: function() {
									console.log('success');
								}
							});
							uni.setStorage({
								key: 'user_picture',
								data: this.user,
								success: function() {
									console.log('success');
								}
							});
							uni.switchTab({
							    url: '../index/indexPage'
							});
						}
						else
						{
							alert("密码错误");
						}
				    },
					fail: () => {
						alert("网络在开小差，请重试");
					}
				});
			}
		}
	}
</script>

<style>
	.zai-box{
		margin-top: 80px;
		padding: 0 100upx;
		position: relative;
	}
	.zai-logo{
		width: 100%;
		width: 100%;
		height: 310upx;
	}
	.zai-title{
		position: absolute;
		top: 0;
		line-height: 360upx;
		font-size: 68upx;
		color: #fff;
		text-align: center;
		width: 100%;
		margin-left: -100upx;
	}
	.zai-form{
		margin-top: 300upx;
	}
	.zai-input{
		background: #e2f5fc;
		margin-top: 30upx;
		border-radius: 100upx;
		padding: 20upx 40upx;
		font-size: 36upx;
	}
	.input-placeholder, .zai-input{
		color: #94afce;
	}
	.zai-label{
		padding: 60upx 0;
		text-align: center;
		font-size: 30upx;
		color: #a7b6d0;
	}
	.zai-btn{
		background: #ff65a3;
		color: #fff;
		border: 0;
		border-radius: 100upx;
		font-size: 36upx;
	}
	.zai-btn:after{
		border: 0;
	}
	/*按钮点击效果*/
	.zai-btn.button-hover{
		transform: translate(1upx, 1upx);
	}
</style>
