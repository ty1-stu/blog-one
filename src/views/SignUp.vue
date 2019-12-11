<template>
	<div id="bg">
		<router-link to="/index">
			<h2>首页</h2>
		</router-link>
		<div class="login-box">
			<form class="login-form">
				<h2>注册</h2>
				<div class="display-all">
					<div class="iconfont sign-fh">&#xe67b;</div>
					<input type="text" v-model="userDto.mobile" placeholder="账号/手机号" id=" mobile" />
				</div>
				<div class="display-all">
					<div class="iconfont sign-fh">&#xe607;</div>
					<input type="password" placeholder="密码" v-model="userDto.password" />
				</div>
				<div class="display-all">
					<div class="iconfont sign-fh">&#xe6ab;</div>
					<input type="nickname" placeholder="用户昵称" v-model=" userDto.nickname" />
				</div>
				<div class="display-all">
					<div class="iconfont sign-fh">&#xe633;</div>
					<input type="gender" placeholder="性别" v-model="userDto.gender" />
				</div>
				<!-- <input type="datetime-local" class="" placeholder="YYYY-MM-DD" v-model="userDto.birthday" > -->
				<input type="button" class="btn btn-lg dark-fill shadow" value="注册" @click="signUp(userDto)" />
				<br><br>
				<router-link to="/sign-in">已有账号？去登录</router-link>
			</form>
		</div>
	</div>
</template>
<script>
	export default {
		data() {
			return {
				userDto: {
					mobile: '',
					password: '',
					nickname: '',
					gender: '',


				}
			};
		},
		created() {
			var number = Math.ceil(Math.random() * 10);
			this.codeUrl = this.GLOBAL.baseUrl + '/code?num = ' + number;
		},
		methods: {
			signUp(userDto) {
				this.axios({
					method: 'post',
					url: this.GLOBAL.baseUrl + '/user/sign-up',
					data: JSON.stringify(this.userDto)
				}).then(res => {
					if (res.data.msg === '成功') {
						alert('注册成功');
						this.$router.push('/sign-in');
					} else {
						alert(res.data.msg);
					}
				});
			}
		}
	};
</script>
<style scoped>
	@font-face {
	  font-family: 'iconfont';  /* project id 1552110 */
	  src: url('//at.alicdn.com/t/font_1552110_bjyv9ut473t.eot');
	  src: url('//at.alicdn.com/t/font_1552110_bjyv9ut473t.eot?#iefix') format('embedded-opentype'),
	  url('//at.alicdn.com/t/font_1552110_bjyv9ut473t.woff2') format('woff2'),
	  url('//at.alicdn.com/t/font_1552110_bjyv9ut473t.woff') format('woff'),
	  url('//at.alicdn.com/t/font_1552110_bjyv9ut473t.ttf') format('truetype'),
	  url('//at.alicdn.com/t/font_1552110_bjyv9ut473t.svg#iconfont') format('svg');
	}

	.iconfont {
		font-family: "iconfont" !important;
		font-size: 18px;
		font-style: normal;
		color: #2196F3;
		-webkit-font-smoothing: antialiased;
		-webkit-text-stroke-width: 0.2px;
		-moz-osx-font-smoothing: grayscale;
	}

	#bg {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: url("../assets/img/注册背景图.jpg");
		background-repeat: no-repeat;
		background-size: 100% 100%;
		background-position: center;
	}

	.login-box {
		width: 350px;
		height: 400px;
		border-radius: 10px;
		background: rgba(255, 255, 255, 0.3);
		position: absolute;
		top: 80px;
		left: 70%;
	}

	.login-form {
		padding-top: 20px;
		width: 70%;
		margin: 0 auto;
		text-align: center;
	}

	.login-form input {
		width: 80%;
		height: 35px;
		outline: none;
		border: none;
		margin-left: 10px;
	}

	.sign-fh {
		width: 30px;
		height: 100%;
		background-color: #EEEEEE;
		border-radius: 10px;
		align-items: center;
	}
</style>
