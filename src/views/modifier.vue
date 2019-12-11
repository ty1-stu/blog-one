<template>
	
</template>

<script>
	export default {
		data(){
			return {
				users: [],
				userDto: {
					id:'',
					nickname:'',
					password:'',
					introduction:''
				}
			}
		},
		created() {
			this.axios.get(this.GLOBAL.baseUrl + '/user').then(res => {
				 console.log(res.data);
				this.users = res.data;
			});
		},
		methods: {
			connect(userDto) {
				this.axios({
					method: 'post',
					url: this.GLOBAL.baseUrl + '/user/change',
					data: JSON.stringify(this.userDto)
				}).then(res => {
					if (res.data.msg === '成功') {
						alert('更改成功');
						this.$router.push('/personal');
					} else {
						alert(res.data.msg);
					}
				});
			}
		}
	}
</script>

<style>
</style>
