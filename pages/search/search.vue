<template>
	<view class="white_panel">
		<zy-search :is-focus="false" :theme="themeClass" :show-want="true" :speechEngine="speechEngine" :hot-list="hotList"
		 @getSearchText="getSearchText"></zy-search>
	</view>
</template>

<script>
	import zySearch from '../../components/zy-search/zy-search.vue'
	export default {
		components: {
			zySearch
		},
		data() {
			return {
				themeClass: 'block',
				speechEngine: 'baidu', //语音识别引擎
				hotList: ['鸡肉', '土豆', '凉面', '黄焖鸡'] //初始化推荐列表
			}
		},
		methods: {
			getSearchText(e) {
				uni.showToast({
					title: '回调的搜索信息: ' + e,
					icon: "none"
				});
				
				try {
				    uni.setStorageSync('search_value', e);
				} catch (e) {
				    // error
				}
				
				// 异步会导致下一个界面出问题
				// uni.setStorage({
				// 	key: 'search_value',
				// 	data: e,
				// 	success: function() {
				// 		console.log('success');
				// 	}
				// });

				uni.switchTab({
					url: '/pages/index/search_1',
					animationType: 'pop-in',
					animationDuration: 200
				});
			}
		},
	}
</script>

<style>
</style>
