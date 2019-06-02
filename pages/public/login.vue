<template>
	<view class="container">
		<!--logo层-->
		 <view class="logo-floor">
			<view class="logo"><image class="logo-img" src="../../static/image/logo.png"/></view>
		 </view>
		 <view class="input-content">
			  <view class="input-item">
				  <view class="input_"><input type="text" class="inpt" id="account"  placeholder="请输入账号" v-model="user.account" @input="inputChange"/></view>
				  <view id="accountClear" class="input-btn" v-show="accountBtnSee" v-on:click="clearInput">X</view>
			  </view>
			  <view class="input-item">
			  		<view class="input-password"><input type="text" id="password" class="inpt"  placeholder="请输入密码" v-model="user.password" @input="inputChange"/></view>
					<view id="passClear" class="input-pas-btn" v-show="passBtnSee" v-on:click="clearInput">X</view>		
					<view class="pas-wjmm">忘记密码</view>	
			  </view>
		 </view>
		 <view class="login-box">
			 <view class="login-btn" v-bind:class="isActive?'login-btn-on':''" @click="toLogin" :disabled="logining">登录</view>
		 </view>
		 <view class="register-box">
			 <view class="message-login">短信验证码登录</view>
			 <view class="rigister">注册</view>
		 </view>
	</view>
</template>

<script>
	import {  
	    mapMutations  
	} from 'vuex';
	export default{
		data(){
			return {
				headerPosition:"fixed",
				headerTop:"0px",
				accountBtnSee:false,
				passBtnSee:false,
				user:{
					account:"",
					password:""
				},
				isActive:false
			}
		},
		onLoad(options){
			// #ifdef H5
			this.headerTop = document.getElementsByTagName('uni-page-head')[0].offsetHeight+'px';
			// #endif
		},
		methods:{
			...mapMutations(['login']),
			inputChange(e){
				var val = e.detail.value;
				if(typeof(val) != "undefined" && val != ""){
					var id = e.currentTarget.id;
					if("account" == id){
						this.accountBtnSee = true;
					}else{
						this.passBtnSee = true;
					}
					//登录按钮开关
					if(this.user.account != "" && this.user.password != ""){
						this.isActive = true;
					}else{
						this.isActive = false;
					}
				}else{
					this.accountBtnSee = false;
					this.passBtnSee = false;
					this.isActive = false;
				}
			},
			clearInput(e){
				var id = e.currentTarget.id;
				if(typeof(id) != "undefined" && id != ""){
					if("accountClear" == id){
						this.user.account = "";
						this.accountBtnSee = false;
					}else{
						this.user.password = "";
						this.passBtnSee = false;
					}
					this.isActive = false;
				}
			},
			async toLogin(){
				if(this.isActive){
					this.logining = true;
				}else{
					return;
				}
				/* 数据验证模块
				if(!this.$api.match({
					mobile,
					password
				})){
					this.logining = false;
					return;
				}
				*/
				const result = await this.$api.json('userInfo');
				if(result.status === 1){
					this.login(result.data);
			        uni.navigateBack();  
				}else{
					this.$api.msg(result.msg);
					this.logining = false;
				}
			}
		}
	}
</script>

<style>
 .content{
		background: $page-color-base;
		padding-top: 96upx;
	}
.logo-floor{
	width: 100%;
	height: auto;
	float: left;
}
.logo-floor .logo{
	width: 120upx;
	height: 120upx;
	margin: 0 auto;
}
.logo-img{
	width: 120upx;
	height: 120upx;
	border-radius:50%
}
.input-content{
	margin-top: 20upx;
	width: 100%;
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
}
.input-content .input-item{
	display: flex;
	flex-flow: row;
	justify-content: flex-start;
	width: 100%;
	margin-top: 80upx;
	margin-left: 10upx;
	margin-right: 10upx;
	border-bottom: 1px solid #FFAA0E;
	
}
.input-item .input_{
	flex: 1;
}
.input-item .input-password{
	flex: 1;
}
.input-item .input-btn{
	float: left;
	width: 60upx;
	text-align: center;
	margin-right: 3upx;	
}
.input-item .input-pas-btn{
	float: right;
	width: 60upx;
	text-align: center;
}
.input-item .pas-wjmm{
	float: right;
	width: 170upx;
	font-size: 26upx;
	text-align: right;
}

.inpt {
  padding-left: 20upx;
  border: none 0;
  outline: medium;
  background: transparent;
  -webkit-appearance: none;
  -moz-appearance: none;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  -moz-tap-highlight-color: transparent;
}
.login-box{
	display: flex;
	flex-flow: row nowrap;
	margin-top: 80upx;
	justify-content: center;
}
.login-box .login-btn{
	width: 85%;
	background-color: #FFD465;
	height: 86upx;
	text-align: center;
	line-height: 86upx;
	border-radius: 43upx;
	color: #F8F8F8; 
}
.login-box .login-btn-on{
	background-color: #FFAA0E;
}
.register-box{
	display: block;
    width: 100%;
	margin-top: 40upx;
	font-size: 30upx;
}
.register-box .message-login{
	display: block;
	float: left;
	margin-left: 35upx;;
}
.register-box .rigister{
	display: block;
	float: right;
	margin-right: 35upx;
}
	
</style>
