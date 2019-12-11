<template>
	<div>
		<div class="row">
			<div v-for="(item, index) in users" :key="index" class="col-3">
				<div class="card shadow flex flex-top-y">
					<div class="card-head flex flex-center">
						<p class="title">{{ item.nickname }}</p>
							<router-link :to="{ path: '/user/' + item.id }">
						<img :src="item.avatar" />
						</router-link>
					</div>
					<div class="card-body flex flex-left">
						<p class="sub-title">{{ item.introduction }}</p>
						<p class="meta">
							<strong>来自：{{ item.address }}</strong>
						</p>
						<p class="meta">{{ item.articles }}篇文章，{{ item.fans }}个粉丝</p>
					</div>
					<div><a :href="item.homepage" class="link" @click="go(item.homepage)">个人主页</a></div>
				</div>
			</div>
		</div>
		<div class="row"><button class="btn btn-lg btn-rd dark-fill jiazai" @click="loadMore">点击加载更多</button></div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			users: [],
			currentPage: 1,
			count: 8
		};
	},
	created() {
		this.axios
			.get(this.GLOBAL.baseUrl + '/user', {
				params: {
					page: this.currentPage,
					count: this.count
				}
			})
			.then(res => {
				console.log(res.data.data.length);
				this.users = res.data.data;
			});
	},
	methods: {
		loadMore() {
			this.currentPage = this.currentPage + 1;
			this.axios
				.get(this.GLOBAL.baseUrl + '/user', {
					params: {
						page: this.currentPage,
						count: this.count
					}
				})
				.then(res => {
					console.log(res.data.data.length);
					let temp = [];
					temp = res.data.data;
					for (var i = 0; i < temp.length; i++) {
						this.users.splice(this.currentPage * this.count, 0, temp[i]);
					}
					console.log(this.users.length);
				});
		},
		go(page) {
			window.location.href = page;
		}
	}
};
</script>
<style scoped>
	.jiazai{
		width: 100%;
	}
.card {
	width: 90%;
	height: 300px;
	background-image: url(../assets/img/4.jpg);
	background-size: 100%, 100%;
	background-repeat: no-repeat;
	margin-bottom: 50px;
	padding: 20px;
}
.card-head {
	height: 30%;
	padding: 10px;
	display: flex;
	justify-content: center;
	align-items: center;
}
.card-head img {
	width: 80px;
	height: 80px;
	border-radius: 50%;
	margin-left: 20px;
}
.card-body {
	width: 80%;
	margin: 0 auto;
}
.card-body > p {
	line-height: 30px;
}
.card a {
	color: rgb(0, 98, 89);
	font-weight: 700;
}
</style>
