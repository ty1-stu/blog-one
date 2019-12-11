<template>
	
		<div class="wrap">
				<div class="says">
					<h1>来，说说你在做什么，想什么...</h1>
					<textarea>请点击发布试试吧</textarea><input type="button"  value="发布">
					<div class="errmsg">请填写内容后再发布！</div>
				</div>
				<ul>
					 
				</ul>
			</div>
	
</template>

<script>
	import $ from '../assets/js/test.js'
export default {
	data() {
		return {
			articles: [],
			users: [],
			topics: []
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
	}
};
</script>

<style>
/* @import url("../assets/css/my.css"); */
</style>
