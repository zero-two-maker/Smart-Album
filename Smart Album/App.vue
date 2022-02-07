<script>
	export default {
		globalData: {  
		    weburl: 'http://192.168.31.91:8080/'  
			//weburl: 'http://172.20.10.2:8080/'
		},
		onLaunch: function() {
			console.log('App Launch');
			uni.login({
			  provider: 'weixin',
			  success: function (loginRes) {
			    console.log(loginRes.authResult);
			    // 获取用户信息
			    uni.getUserInfo({
			      provider: 'weixin',
			      success: function (infoRes) {
			        console.log('用户昵称为：' + infoRes.userInfo.nickName);
			      }
			    });
			  }
			});
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import url("static/css/public.css");
</style>
