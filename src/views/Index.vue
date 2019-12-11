<template>
	<div>
		<!-- <div class="row">
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
		</div> -->
		<div class="row">
			<!-- 主页轮播 -->
			<div class="head">
				<div class="carousel-wrap">
					<transition-group tag="ul" class="slide-ul" name="slide">
						<li v-for="(art,index) in avatars" :key="index" v-show="index===currentIndex" @mouseenter="stop" @mouseleave="go">
							<img :src="art">
							<div class="welldown">
								<div class="a-color">
									Well...A double rainbow is a phenomenon of optics that displays a spectrum of light
									due to the sun shining onto droplets of moisture in the atmosphere… Does that explain it?
									<hr>
									好的……双重彩虹，指的是阳光在透过大气层的湿气时因为折射和反射而出现两道不同彩虹的现象……各位听懂了么？
								</div>
								</br>
								<div class="a-color name-pos">------------光辉女郎-拉克丝</div>
							</div>
						</li>
					</transition-group>
					<div class="carousel-items">
						<span v-for="(art,index) in avatars" :class="{active:index===currentIndex}" @mouseover="change1(index)" />
					</div>
				</div>
			</div>
		</div>
		<div class="row">
			<div class="col-8">
				<h3>热门文章</h3>
				<div v-for="(item, index) in articles" :key="index" class="col-12">
					<div class="media-wraaper border">
						<div class="media-left">
							<router-link :to="{ path: '/user/' + item.article.userId }">
								<img :src="item.author.avatar" class="avatar-lg link" />
							</router-link>
							<p>{{ item.author.nickname }}</p>
							<strong>来自</strong>
							<p>{{ item.topic.topicName }}</p>
						</div>
						<div class="media-middle flex flex-around flex-left">
							<router-link :to="{ path: '/article/' + item.article.id }" class="subtitle">
								{{ item.article.title }}
							</router-link>
							<p class="sub-title link">{{ item.article.summary }}</p>
							<el-badge :value="item.article.comments" class="item" type="primary">
								<router-link to="/message">
								<el-button size="small">评论</el-button>
								</router-link>
							</el-badge>

							<el-badge :value="item.article.likes" class="item" type="warning">

								<el-button size="small">喜欢</el-button>
							</el-badge>
							<router-link :to="{ path: '/article/' + item.article.id }">
								<el-button :plain="true" @click="open" size="small" class="item">查看原文>></el-button>
							</router-link>
						</div>
						<div class="media-right"><img :src="item.article.thumbnail" alt="" /></div>
					</div>
				</div>
			</div>
			<div class="col-4">
				<!-- 音乐 -->
				<video class="music-size" controls="" autoplay="" name="media">
					<source src="http://up_mp4.t57.cn/2018/1/03m/13/396131213056.m4a" type="audio/mp4">
				</video>
				<h3>热门作者</h3>
				<div v-for="(item, index) in users" :key="index" class="row">
					<div class="col-12 border box">
						<div class="flex-center-y">
							<router-link :to="{ path: '/user/' + item.id }">
								<img :src="item.avatar" class="avatar-xs link" />
							</router-link>
							<p class="sub-title">{{ item.nickname }}</p>
						</div>
						<div class="flex-center-y">
							<p class="meta">{{ item.fans }}个粉丝</p>
							<p class="meta">写了{{ item.articles }}篇文章</p>
						</div>
						<div class="flex-center-y">
							<!-- <button class="btn btn-follow @click=change" :disabled="btnDisabled" :class="{
							'btn-disabled': !switchCss,
							'btn-normal': switchCss
						}">
						</button> -->


							<button v-if="item.iscare=== 0" @click="care(item)" class="carebtn">
								<i class="iconfont">&#xe62e;</i> {{msg}}</button>

							<button v-if="item.iscare ===1" @click="care(item)" class="btn-change">
								<i class="iconfont">&#xe600;</i> 已关注</button>

						</div>

					</div>
				</div>

				<h3>热门音乐</h3>
				<div class="col-12 border box">
					<div>
						<ul class="u-flex">
							<li class="bottom-size bottom-color avatar">
								<bottom>
									<router-link to="/message">下山</router-link>
								</bottom>
							</li>

							<li class="bottom-size2 bottom-color2 avatar v-c">

								<bottom>
									<router-link to="/message">it's very good</router-link>
								</bottom>
							</li>

						</ul>
						<ul class="u-flex1">
							<li class="bottom-size1 bottom-color1 avatar v-c">
								<bottom>
									<router-link to="/message">vue</router-link>
								</bottom>
							</li>
							<li class="bottom-size1 bottom-color avatar v-c">
								<bottom>
									<router-link to="/message">css</router-link>
								</bottom>
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
				articles: [],
				users: [],
				avatars: [
					"https://godweiyang.com/medias/featureimages/13.jpg",
					"https://www.mapblog.cn/img/banner/five.jpeg",
					"https://www.mapblog.cn/img/banner/one.jpeg",
					"https://www.mapblog.cn/img/banner/two.jpeg",
					"https://www.mapblog.cn/img/banner/four.jpeg",
				],
				currentIndex: 0,
				timer: null,
				iscare: false,
				user: JSON.parse(localStorage.getItem('user')),
				topics: [],
				msg: "关注"
			};
		},
		created() {
			this.axios.get(this.GLOBAL.baseUrl + '/article').then(res => {
				// console.log(res.data.data);
				this.articles = res.data.data;
			});
			this.axios.get(this.GLOBAL.baseUrl + '/user').then(res => {
				// console.log(res.data.data);
				this.users = res.data.data;
			});

			this.axios.get(this.GLOBAL.baseUrl + '/topic').then(res => {
				// console.log(res.data.data);
				this.topics = res.data.data;
			});
			/* 动画自动播放 */
			this.$nextTick(() => {
				this.timer = setInterval(() => {
					this.autoPlay()
				}, 3000)
			});
		},
		methods: {
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
				if (item.iscare == 0) {
					item.iscare = 1;
				} else {
					item.iscare = 0;
				}
				this.axios.post(this.GLOBAL.baseUrl + '/user', {
						params: {
							id: "12",
							iscare: item.iscare
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
	.music-size {
		position: fixed;
		width: 100px;
		height: 40px;
		right: -60px;
		top: 100px;
		border-radius: 20px;
		box-shadow: 0px 0px 15px #2196F3;
	}

	@font-face {
		font-family: 'iconfont';
		/* project id 1552110 */
		src: url('//at.alicdn.com/t/font_1552110_jvzcmzspo1j.eot');
		src: url('//at.alicdn.com/t/font_1552110_jvzcmzspo1j.eot?#iefix') format('embedded-opentype'),
			url('//at.alicdn.com/t/font_1552110_jvzcmzspo1j.woff2') format('woff2'),
			url('//at.alicdn.com/t/font_1552110_jvzcmzspo1j.woff') format('woff'),
			url('//at.alicdn.com/t/font_1552110_jvzcmzspo1j.ttf') format('truetype'),
			url('//at.alicdn.com/t/font_1552110_jvzcmzspo1j.svg#iconfont') format('svg');
	}

	.iconfont {
		font-family: "iconfont" !important;
		font-size: 16px;
		font-style: normal;
		-webkit-font-smoothing: antialiased;
		-webkit-text-stroke-width: 0.2px;
		-moz-osx-font-smoothing: grayscale;
	}

	.carebtn {
		border: none;
		background-color: rgb(26, 160, 52);
		cursor: pointer;
		width: 80px;
		height: 30px;
		color: #FFFFFF;
		border-radius: 20px;
	}

	.btn-change {
		border: none;
		background-color: rgb(238, 238, 238);
		cursor: pointer;
		color: #FFFFFF;
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

	.item {
		margin-top: 10px;
		margin-right: 40px;
	}
</style>
