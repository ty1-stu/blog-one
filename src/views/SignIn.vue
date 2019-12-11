<template>
	<div id="bg">
		<router-link to="/">
			<h2>首页</h2>
		</router-link>
		<div class="login-box">
			<form class="login-form">
				<h2>登陆</h2>
				<div class="display-all">
					<div class="iconfont sign-fh">&#xe67b;</div>
					<input type="text" placeholder="账号/手机号" v-model="userDto.mobile" id="mobile" />
				</div>
				<div class="display-all">
					<div class="iconfont sign-fh">&#xe607;</div>
					<input type="password" placeholder="密码" v-model="userDto.password" />
				</div>
				<div class="code-box">
					<input type="text" placeholder="验证码" v-model="userDto.code" class="code"/>
					<img class="verify" @click.prevent="refresh" ref="codeImg" />
				</div>
				<input type="button" class="btn btn-lg dark-fill shadow" value="登录" @click="signIn(userDto)" />
				<br><br>
				<router-link to="/sign-up">没有账号？去注册</router-link>
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
					code: ''
				},
				token: ''
			};
		},
		created() {
			this.axios.get(this.GLOBAL.baseUrl + '/code', {
				responseType: 'blob'
			}).then(res => {
				// console.log(res);
				var img = this.$refs.codeImg;
				let url = window.URL.createObjectURL(res.data);
				img.src = url;
				console.log(res.headers);
				//取得后台通过响应头返回的sessionId的值
				this.token = res.headers['access-token'];
				console.log(this.token);
			});
		},
		methods: {
			signIn(userDto) {
				this.axios({
					method: 'post',
					url: this.GLOBAL.baseUrl + '/user/sign-in',
					data: JSON.stringify(this.userDto),
					headers: {
						'Access-Token': this.token //将token放在请求头带到后端
					}
				}).then(res => {
					if (res.data.msg === '成功') {
						alert('登录成功');
						localStorage.setItem('user', JSON.stringify(res.data.data));
						this.$router.push('/');
					} else {
						alert(res.data.msg);
						this.userDto.code = '';
					}
				});
			},
			refresh() {
				this.axios.get(this.GLOBAL.baseUrl + '/code', {
					responseType: 'blob'
				}).then(res => {
					// console.log(res);
					var img = this.$refs.codeImg;
					let url = window.URL.createObjectURL(res.data);
					img.src = url;
					console.log(res.headers);
					//取得后台通过响应头返回的sessionId的值
					this.token = res.headers['access-token'];
					console.log(this.token);
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
		background-image: url(../assets/img/登陆背景图.jpg);
		background-size: 100%, 100%;
	}

	.login-box {
		width: 350px;
		height: 400px;
		border: 2px solid rgb(73, 137, 244);
		border-radius: 10px;
		background-color: rgb(73, 137, 244, 0.6);
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

	.code-box {
		display: flex;
		margin-top: 10px;
		margin-bottom: 20px;
		justify-content: space-between;
	}

	.verify {
		flex: 0 0 40%;
		height: 35px;
	}

	.verify:hover {
		cursor: pointer;
	}

	.code {
		flex: 0 0 45%;
		height: 35px;
		margin-left: 20px;
	}

	a {
		color: #008b8b;
		font-weight: 700;
	}

	.sign-fh {
		width: 30px;
		height: 100%;
		background-color: #EEEEEE;
		border-radius: 10px;
		text-align: center;
	}
</style>
