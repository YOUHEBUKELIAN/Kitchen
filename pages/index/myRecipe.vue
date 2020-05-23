<template name="page2">
	<view class="pageView bg-purple">
			<view v-for="item in items" :key="item.name">
				<view class="uni-padding-wrap uni-common-mt" style="background-color:#808080" >
					<view class="uni-flex uni-row">
						<view class="text uni-flex" style="width: 200rpx;height: 220rpx;-webkit-justify-content: center;justify-content: center;-webkit-align-items: center;align-items: center;">
							<image :src="item.cover" style="width: 150rpx;height: 150rpx;"></image>
						</view>
						<view class="uni-flex uni-column" style="-webkit-flex: 1;flex: 1;-webkit-justify-content: space-between;justify-content: space-between;">
							<view class="text" style="height: 120rpx;text-align: left;padding-left: 20rpx;padding-top: 10rpx;">
								{{item.name}}
							</view>
							<view class="uni-flex uni-row">
								<view class="text" style="-webkit-flex: 1;flex: 1;">{{item.collectionNumber}}</view>
								<view class="text" style="-webkit-flex: 1;flex: 1;">{{item.browseNumber}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
</template>

<script>
	export default {
		name: "page2",
		data() {
			return {
				items:[{
					cover:"123",
					name:"465",
					collectionNumber:7,
					browseNumber:8
				}]
			}
		},
		onLoad() {

		},
		onShow() {
			uni.switchTab({
				url: "../index/index",
				"open-type": "switchTab"
			})
		},
		onLoad(e) {
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
	.example {
		padding: 0 15px 15px;
	}
	
	.example-info {
		padding: 15px;
		color: #3b4144;
		background: #ffffff;
	}
	
	.example-body {
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
		padding: 0;
		font-size: 14px;
		background-color: #ffffff;
	}
</style>
