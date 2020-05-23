<template name="page2">
	<view class="pageView bg-purple">Page 1</view>
</template>

<script>
	export default {
		name: "page2",
		data() {
			return {

			}
		},
		onLoad() {

		},
		onShow() {
			console.log(e)
			uni.switchTab({
				url: "../index/index",
				"open-type": "switchTab"
			})
		},
		onLoad(e) {
			console.log(e.id);
			uni.request({
				url: 'http://pope.utools.club/findMyRecipe',
				data: {
					nickname: e.nickname
				},
				method: 'POST',
				header: {
					'content-type': 'application/x-www-form-urlencoded',
				},
				success: (ret) => {
					console.log(ret.data)
					this.list = ret.data.data
				}
			});
		},
		methods: {

		}
	}
</script>

<style>
</style>
