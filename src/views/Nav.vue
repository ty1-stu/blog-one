<template>
	<div :class="{red: !isshow,blue: isshow}" >
				
					
		<div class="nav primary-fill shadow top-card">
			<div class="nav-bar">
				<ul class="nav-list flex-around">
					<router-link to="/message">
					<li>
						<img src="../assets/img/Yi-logo.jpg" class="gif-size">
					</li></router-link>
					<li class="nav-item"><router-link to="/index">主页</router-link></li>
					<li class="nav-item"><router-link to="/topics">专题</router-link></li>
					<li class="nav-item"><router-link to="/articles">文章</router-link></li>
					<li class="nav-item"><router-link to="/users">作者</router-link></li>
					<li class="nav-item"><input type="text" class="input-box" placeholder="Search Google or type a URL" v-model="keywords" /></li>
					<li class="nav-item"><button class="btn btn-lg btn-rd dark-border btn-color" @click="search" >搜索</button></li>
					<li class="nav-item" v-if="!this.user"><router-link to="/sign-in">登录</router-link></li>
						<router-link :to="{ path: '/user/' + user.id }" v-if="this.user"><img :src="user.avatar" @mouseenter="this.show = true" class="avatar-xs abs-center-right" /></router-link>
						
						<li class="nav-item">
							<router-link to="/send" v-if="this.user">发文章</router-link>
							</li>
					<li class="nav-item" v-if="this.user"><a class="link" @click="logout">退出</a></li>
					<li class="nav-item"><button class="btn btn-lg btn-rd dark-border btn-color" @click="isshow=!isshow">点击有惊喜</button></li>
				</ul>
			</div>
			</div>
		
		<router-view class="container" />
	</div>
</template>
<script>
export default {
	data() {
		return {
			user: JSON.parse(localStorage.getItem('user')),
			keywords: '',
			isshow: true,
		};
	},
	created() {},
	methods: {
		logout() {
			this.user = null;
			localStorage.clear();
		},
		search() {
			let currentPath = this.$route.path;
			alert(currentPath);
			if (currentPath != '/search' || currentPath != '/search/article' || currentPath != '/search/topic' || currentPath != '/search/usere') {
				this.$router.push({ path: '/search', query: { keywords: this.keywords } });
			} else {
				this.$router.push({ path: '/empty', query: { keywords: this.keywords } });
			}
		}
	}
};
</script>
<style scoped>
/* 路由激活高亮样式 */
.router-link-active {
	background-color: rgba(0, 0, 0, 0.35);
	font-weight: 700;
	border-radius: 50px;
}
.top-card {
	background-image: url(../assets/img/顶部导航栏.jpg);
	
	background-repeat: no-repeat;
	background-size:100% 100% ;
	 background-position: center;
}
.container {
	margin-top: 80px;
}
.gif-size{
	height: 60px;
	width:60px;
	border-radius: 50%;
}
.router-link-active {
		background-color: rgba(0, 0, 0, 0);
		font-weight: 700;
	}
	.container {
		margin-top: 80px;
	}
		.red {
	      background-image: linear-gradient(#9198e5,#e66465);
	      color: #DDDDDD;
	    }
	/* 	.primary-fill {
		background: linear-gradient(to bottom, rgb(218, 60, 180), rgb(108, 149, 218));;
			color: #fff;
		}
		.middle-fill {
		  background: linear-gradient(to bottom, rgb(255, 101, 144), rgb(123, 110, 157));;
		  	color: #DDDDDD;
		} */
		.nav-item {
			height: 70px;
			line-height: 70px;
		}
		
		.nav-item input {
			margin-top: 15px;
			width: 200px;
			height: 35px;
		}
		
		.nav-item img {
			width: 45px;
			height: 45px;
			border-radius: 50%;
		}
		
		.nav-item a {
			display: inline-block;
			width: 80px;
			height: 35px;
			margin-right: 5px;
			font-size: 18px;
			font-weight: 600;
			color: #fff;
			text-align: center;
			line-height: 35px;
			letter-spacing: 3px;
		}
		.fu{
			margin-left: 90px;
		}
       
     .btn-color{
	   background-color: #EEEEEE;
	   
      }
</style>
