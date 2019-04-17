<template>
	<view class="container">
		<input class="uni-input" password type="text" placeholder="输入密码" v-model="password" required="required" />
		<button class="green-btn" @tap="signUp(userDTO)">注册</button>
	</view>
</template>

<script>
export default {
	data() {
		return {
			  userDTO: {
				mobile: '',
				password: ''
			},
			  count:"",//倒计时
		};
	},
/* created(){
​  this.threeGo()
}, */
onLoad: function(option) {
		//option为object类型，会序列化上个页面传递的参数
		console.log(option.mobile);
		this.mobile = option.mobile;
	},
	methods: {
		signUp: function(userDTO) {
			var _this = this;
			uni.request({
				url: this.apiServer + '/user/sign_up',
				method: 'POST',
				header: { 'content-type': 'application/json' },
				data: {
					mobile: _this.mobile,
					password: _this.password
				},
				success: res => {
				  	if (res.data.code === 0) {
	                uni.showModal({
						title: '提示',
						content: '注册成功'
					})
					uni.navigateTo({
						url: '../signin/signin'
						});
					} else {
						uni.showModal({
							title: '提示',
							content: res.data.msg
						});
					}
				}
			});
		},
		setTimeout: function() {		
			 const TIME_COUNT = 3;		
			 if(!this.timer){			
				 this.count = TIME_COUNT;			
				 this.show = false;			
				 this.timer = setInterval(()=>{			
					 if(this.count > 0 && this.count <= TIME_COUNT){				
						 this.count--;
						}else{				
						this.show = true;				
						clearInterval(this.timer);				
						this.timer = null;				
						//跳转的页面写在此处				
						this.$router.push({					
							path: '/signin/signin',
							});			
							}		 
							 },1000)		
		} 
		}
	
	//3秒后进入跳转页面    
	}
};
</script>

<style>
	input {
		height: 50px;
		border-bottom: 1px solid #eee;
		margin-bottom: 5px;
	}
</style>