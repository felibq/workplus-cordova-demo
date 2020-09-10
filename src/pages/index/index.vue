<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view>
			<text class="title" v-if="userInfo.name">{{userInfo.name}},欢迎你</text>
			<text class="title" v-if="!userInfo.name">请使用APP访问本页面</text>
		</view>
	</view>
</template>

<script>
	//引入 w6s-js-sdk
	import * as w6s from '../../../node_modules/@w6s/sdk/dist/sdk.min.js';
	export default {
		data() {
			return {
				apiUrl : '',
				userInfo:{}
			}
		},
		onLoad() {
			this.getApiUrl();
			this.getUserInfo();
		},
		methods: {
			getUserInfo(){
				let _this = this;
				// 使用w6s来访问js-sdk
				w6s.user.getCurrentUserInfo({
					needEmpInfo: true,
					success: function(res) {
						console.log("当前用户:" + res.name);
						_this.userInfo = res;
					},
					fail: function(err) {},
				});
			},
			getApiUrl(){
				let _this = this;
				// 如果你需要使用的接口不包含在 JS-SDK 内，但又想获得 JS-SDK 一样的接口调用体验，可以使用下方接口进行包装转换：
				// 带回调的方法，setTimer 为 boolean类型，表示是否启用定时器
				// w6s.exec(service, action, args, success, fail, setTimer);
				const getApiUrl = w6s.exec('WorkPlus_Auth', 'getServerInfo', ['api_url']);
				getApiUrl.then(
					function(res){
						console.log('apiUrl:' + res.api_url);
						_this.apiUrl = res.api_url;
					}
				).catch(
					function(){

					}
				)
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin: 200rpx auto 50rpx auto;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
