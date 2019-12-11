<template>
	<div class="container">
		<div class="banner flex flex-center"><img :src="userVo.user.banner" class="cover shadow" /></div>
		<div class="info">
			<img :src="userVo.user.avatar" class="avatar" />
			<p>{{ userVo.user.nickname }}</p>
			<div class="low">
				<hr>
			<router-link to="/modifier">修改个人信息</router-link>
			</div>
			<!-- <p>{{ userVo.user.introduction.slice(0, 20) }}...</p> -->
		</div>
		
		<div class="row">
			<div class="col-8">
				<div class="col-12" v-for="(item, index) in userVo.articleList" :key="index">
					<div class="media-wraaper shadow">
						<div class="media-left"><img :src="item.article.thumbnail" class="thumnail-xs" /></div>
						<div class="media-middle">
							<p class="title">{{ item.article.title }}</p>
							<p class="sub-title">{{ item.article.summary }}</p>
						</div>
					 <el-button
					    plain
					    @click="open1">
					    删除
					  </el-button>

						
						
					</div>
				</div>
			</div>
			<div class="col-4">
				<h3>热门作者</h3>
				<div class="col-3" v-for="(item, index) in topics" :key="index">
					<div class="card link shadow">
						<router-link :to="{ path: '/topic/' + item.id }"><img :src="item.logo" class="logo" /></router-link>
						<p class="title">{{ item.topicName }}</p>
						<p class="sub-title">{{ item.description.slice(0, 20) }}...</p>
						<p class="meta">
							<span class="gutter">{{ item.articles }}篇文章,</span>
							<span>{{ item.follows }}人关注</span>
						</p>
					</div>
				</div>
				
				
				
				
				<h3>热门评论</h3>
				<div class="col-12 border box">
					<div>
						<ul class="u-flex">
							<li class="bottom-size bottom-color avatar">
								<bottom><router-link to="/message">express</router-link></bottom>
							</li>
							
							<li class="bottom-size2 bottom-color2 avatar v-c">
								
								<bottom><router-link to="/message">it's very good</router-link></bottom>
							</li>
							
						</ul>
						<ul class="u-flex1">
							<li class="bottom-size1 bottom-color1 avatar v-c">
								<bottom><router-link to="/message">vue</router-link></bottom>
							</li>
							<li class="bottom-size1 bottom-color avatar v-c">
								<bottom><router-link to="/message">css</router-link></bottom>
							</li>
						</ul>
					</div>
					
					
				</div>
				
				
			</div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			userVo: {
				user: {},
				articleList: {}
			},
			users: [],
			topics: [],
			visible: false,
		};
	},
	created() {
		var id = this.$route.params.id;
		this.axios.get(this.GLOBAL.baseUrl + '/user/' + id).then(res => {
			console.log(res.data.data);
			this.userVo = res.data.data;
		});
		this.axios.get(this.GLOBAL.baseUrl + '/topic').then(res => {
			// console.log(res.data.data);
			this.topics = res.data.data;
		});
	},
	methods: {
		  open1() {
		        this.$notify({
		          title: '删除',
		          message: '删除成功',
		          type: 'success'
		        });
		      },
			change1(index) {
				this.currentIndex = index
			},
			gotoPersonalDetail(id) {
				this.$router.push('/persondeatil?id=' + id);
			},
			gotoArticleDetail(id) {
				this.$router.push('/articledeatil?id=' + id);
			},
			go() {
				this.timer = setInterval(() => {
					this.autoPlay()
				}, 3000)
			},
			stop() {
				clearInterval(this.timer)
				this.timer = null
			},
		    care(item) {
				if(item.iscare == 0){
					item.iscare = 1;
				}else{
					item.iscare=0;
				}
				this.axios.post(this.GLOBAL.baseUrl + '/user', {
										params: {
											id : "12",
											iscare : item.iscare
										},
									})
	
				.then(res => {
					// console.log(res.data.data);
					this.users = [];
					this.users = res.data.data;
				});
			  
			   /* this.msg == "关注" ? "已关注": "关注" */
			},
			autoPlay() {
				this.currentIndex++
				if (this.currentIndex > 4) {
					this.currentIndex = 0
				}
			},
		
		}
	};
</script>

<style scoped="scoped">
.low{
	margin-top: 10px;
}
.banner {
	width: 100%;
	height: 300px;
}
.cover {
	width: 100%;
	height: 100%;
	border-radius: 10px;
}
.info {
	position: relative;
	top: -60px;
	left: -400px;
	color: #fff;
	margin-bottom: -50px;
	padding: 10px;
	background-color: rgb(0, 0, 0, 0.3);
	width: 130px;
	height: 130px;
	margin: 0 auto;
	text-align: center;
	border-radius: 20px;
}
.info img {
	width: 80px;
	height: 80px;
	border-radius: 50%;
	border: 2px solid #fff;
	margin-bottom: 10px;
}
.row {
	margin-top: -50px;
}
@font-face {
	  font-family: 'iconfont';  /* project id 1432516 */
	  src: url('//at.alicdn.com/t/font_1432516_ubf5xowpcp9.eot');
	  src: url('//at.alicdn.com/t/font_1432516_ubf5xowpcp9.eot?#iefix') format('embedded-opentype'),
	  url('//at.alicdn.com/t/font_1432516_ubf5xowpcp9.woff2') format('woff2'),
	  url('//at.alicdn.com/t/font_1432516_ubf5xowpcp9.woff') format('woff'),
	  url('//at.alicdn.com/t/font_1432516_ubf5xowpcp9.ttf') format('truetype'),
	  url('//at.alicdn.com/t/font_1432516_ubf5xowpcp9.svg#iconfont') format('svg');
	}
	.iconfont{
	    font-family:"iconfont" !important;
	    font-size:16px;font-style:normal;
	    -webkit-font-smoothing: antialiased;
	    -webkit-text-stroke-width: 0.2px;
	    -moz-osx-font-smoothing: grayscale;
		}
	.carebtn{
	border: none;
	background-color: rgb(26, 160, 52); 
	cursor: pointer;
	width: 80px; 
	height: 30px;
	color: #FFFFFF;
	border-radius: 20px;
	}
	.btn-change{ 
	border: none;
	background-color: rgb(238, 238, 238); 
	cursor: pointer;
	color:#FFFFFF;
	width: 80px; 
	height: 30px;
	border-radius: 20px;
	}
.head {
	width: 100%;
	height: 260px;
	margin-bottom: 20px;
	
}
	
.head img {
	width: 100%;
	height: 100%;
}
.logo {
	border-top-left-radius: 5px;
	border-top-right-radius: 5px;
}
.logo:hover {
	opacity: 0.6;
}
.box {
	display: flex;
	justify-content: space-around;
	height: 80px;
	padding: 10px;
}
.btn-follow {
	background-color: #42c02e;
	font-weight: 400;
	font-size: 15px;
	color: #fff;
	padding: 5px 0;
	width: 80px;
	height: 30px;
	border-radius: 40px;
	display: inline-block;
	text-align: center;
}
.v-c {
	    flex: 0 0 auto;
		text-align: center;
		align-items: center;
		
	}

	.u-flex {
		display: flex;
		margin-left: -120px;
	}

	.u-flex1 {
		display: flex;
		margin-top: 10px;
		margin-left: -120px;
	}

	.avatar {
		width: 100px;
		height: 100px;
		transition: all 1s;

		margin: 0 -20px;
		vertical-align: center;
	}

	.avatar:hover {
		transform: scale(1.1);
		opacity: 0.7;
		cursor: pointer;
	}

	.active {

		color: #00BBDD;
		font-weight: 600;
		/* border-bottom: 2px solid #00BBDD; */
	}

	.bottom-size {
		flex: 0 0 auto;
		margin-left: 30px;
		height: 27px;
		width: 63px;
		border-radius: 5px;
	}

	.bottom-size1 {
		flex: 0 0 auto;
		margin-left: 30px;
		height: 27px;
		width: 45px;
		border-radius: 5px;
	}
	.bottom-size2 {
		flex: 0 0 auto;
		margin-left: 30px;
		height: 27px;
		width: 125px;
		border-radius: 5px;
	}

	.bottom-color {
		background-color: #2196F3;
	}

	.bottom-color1 {
		background-color: rgb(26, 160, 52);
	}

	.bottom-color2 {
		background-color: rgb(220, 75, 62);
	}
</style>
