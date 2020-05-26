<template name="page2">
	<view class="pageView bg-purple">
		<view v-for="item in items" :key="item.name">
			<view class="uni-padding-wrap uni-common-mt" style="background-color:#808080">
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
				items: [{
					cover: "123",
					name: "465",
					collectionNumber: 7,
					browseNumber: 8
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
			console.log(e.nickname);
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
					if (res.data.code == 5) {
						for (var i = 0; i < res.data.data.length; i++) {
							var temp = {
								id: res.data.data[i].id,
								name: res.data.data[i].name,
								cover: res.data.data[i].cover,
								collectionNumber: res.data.data[i].collectionNumber,
								browseNumber: res.data.data[i].browseNumber
							};
							this.items.push(temp);
						}
					} else if (res.data.code == -5) {
						//alert("查询失败")
					}
				}
			});
		},
		methods: {

		}
	}
</script>

<style>
	/* 头条小程序组件内不能引入字体 */
	/* #ifdef MP-TOUTIAO */
	@font-face {
		font-family: uniicons;
		font-weight: normal;
		font-style: normal;
		src: url('~@/static/uni.ttf') format('truetype');
	}

	/* #endif */

	/* #ifndef APP-NVUE */
	page {
		display: flex;
		flex-direction: column;
		box-sizing: border-box;
		background-color: #FFFFFF;
		min-height: 100%;
		height: auto;
	}

	view {
		font-size: 14px;
		line-height: inherit;
	}

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

	/* #endif */
	.example {
		padding: 0 15px;
	}

	.example-info {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
		padding: 15px;
		color: #3b4144;
		background-color: #ffffff;
		font-size: 14px;
		line-height: 20px;
	}

	.example-info-text {
		font-size: 14px;
		line-height: 20px;
		color: #3b4144;
	}


	.example-body {
		flex-direction: column;
		padding: 15px;
		background-color: #ffffff;
	}

	.input-view {
		/* #ifndef APP-PLUS-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		width: 650rpx;

		flex: 1;
		background-color: #f8f8f8;
		height: 30px;
		border-radius: 15px;
		padding: 0 15px;
		flex-wrap: nowrap;
		margin: 7px 0;
		line-height: 30px;
	}

	.input-uni-icon {
		line-height: 30px;
	}

	.nav-bar-input {
		height: 30px;
		line-height: 30px;
		width: 350rpx;
		/* #ifdef APP-PLUS-NVUE */
		/* width: 370rpx; */
		/* #endif */
		padding: 0 5px;
		font-size: 28rpx;
		background-color: #f8f8f8;
	}

	.example-body {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
		padding: 0;
	}

	.flex-item {
		width: 33.3%;
		height: 200rpx;
		text-align: center;
		line-height: 200rpx;
	}

	.flex-item-V {
		width: 100%;
		height: 150rpx;
		text-align: center;
		line-height: 150rpx;
	}

	.text {
		margin: 15rpx 10rpx;
		padding: 0 20rpx;
		background-color: #ebebeb;
		height: 70rpx;
		line-height: 70rpx;
		text-align: center;
		color: #777;
		font-size: 26rpx;
	}

	.desc {
		/* text-indent: 40rpx; */
	}
</style>
