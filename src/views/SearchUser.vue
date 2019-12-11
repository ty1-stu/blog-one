<template>
	<div class="row">
		<div v-for="(item, index) in users" :key="index" class="col-12">
			<div class="media-wraaper shadow">
				<div class="media-left">
					<img :src="item.avatar" class="avatar-lg link" />
					<p>{{ item.nickname }}</p>
				</div>
				<div class="media-middle">
					<p>{{ item.introduction}}</p>
					<p>
						<span class="meta gutter">写了{{ item.articles }}篇文章</span>
						<span class="meta gutter">{{ item.fans }}个粉丝</span>
						<span class="meta gutter">关注了{{ item.follows }}人</span>
					</p>
				</div>
				<div class="media-right">
					<button v-if="item.iscare=== 0" @click="care(item)" class="btn btn-lg btn-rd primary-fill">
						<i class="iconfont">&#xe600;</i>关注</button>
				
				<button v-if="item.iscare ===1" @click="care(item)" class="btn btn-lg btn-rd warning-fill">
					<i class="iconfont">&#xe681;</i>已关注</button>
			</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			users: [],
			keywords: '',
			iscare:false
		};
	},
	created() {
		this.keywords = this.$route.query.keywords;
		console.log(this.keywords);
		this.axios
			.get(this.GLOBAL.baseUrl + '/user', {
				params: {
					keywords: this.keywords
				}
			})
			.then(res => {
				console.log(res.data.data.length);
				this.users = res.data.data;
			});
	},
	methods: {
	
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
		
	
	}
	
};
</script>
<style>
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
	/* .btn-change{
	border: none;
	background-color: rgb(238, 238, 238); 
	cursor: pointer;
	color:#FFFFFF;
	width: 150px; 
	height: 30px;
	border-radius: 20px;
	margin-right: 20px;
	} */
</style>
