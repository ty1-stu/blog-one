<template>
	<div class="row">
		<div v-for="(item, index) in articles" :key="index" class="col-12">
			<div class="media-wraaper bg shadow">
				<div class="media-left">
					<img :src="item.author.avatar" class="avatar-lg link" />
					<p>{{ item.author.nickname }}</p>
					<strong>来自</strong>
					<p>{{ item.topic.topicName }}</p>
				</div>
				<div class="media-middle flex flex-left">
					<router-link :to="{ path: '/article/' + item.article.id }">
						<p>
							<span>{{ item.article.id }}</span>
							{{ item.article.title }}
						</p>
					</router-link>
					<p class="sub-title link">{{ item.article.summary }}</p>
			     <!-- 		<p>
						<span class="meta">{{ item.article.comments }}评论</span>
						<span class="meta">{{ item.article.likes }}喜欢</span>
					</p> -->
					<el-badge :value="item.article.comments" class="item" type="primary">
					  <el-button size="small">评论</el-button>
					</el-badge>
				
					<el-badge :value="item.article.likes" class="item" type="warning">
						
					  <el-button size="small">喜欢</el-button>
					</el-badge>
					<router-link :to="{ path: '/article/' + item.article.id }">
		            <el-button :plain="true" @click="open" size="small" class="item">查看原文>></el-button>
                    </router-link>
				</div>
				<div class="media-right"><img :src="item.article.thumbnail" /></div>
			</div>
		</div>

		<div class="col-12"><button class="btn btn-lg warning-fill" @click="loadMore">点击加载更多</button></div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			articles: [],
			currentPage: 1,
			count: 20
		};
	},
	created() {
		this.axios
			.get(this.GLOBAL.baseUrl + '/article', {
				params: {
					page: this.currentPage,
					count: this.count
				}
			})
			.then(res => {
				console.log(res.data.data.length);
				this.articles = res.data.data;
			});
	},
	methods: {
		loadMore() {
			this.currentPage = this.currentPage + 1;
			this.axios
				.get(this.GLOBAL.baseUrl + '/article', {
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
						this.articles.splice(this.currentPage * this.count, 0, temp[i]);
					}
					console.log(this.articles.length);
				});
		}
	}
};
</script>

<style scoped="scoped">
.bg {
	background-image: url(../assets/img/5.jpg);
	background-size: contain;
	background-repeat: no-repeat;
	background-size:100% 100% ;
}
.item {
  margin-top: 10px;
  margin-right: 40px;
}

</style>
