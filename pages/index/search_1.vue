<template>
	<view>
		<uni-nav-bar class="nav-bar">
			<view class="input-view">
				<uni-icons class="input-uni-icon" type="search" size="22" color="#666666" />
				<input confirm-type="search" class="nav-bar-input" type="text" placeholder="想吃点什么"	@confirm="confirm" @click="toSearch_2">
			</view>
		</uni-nav-bar>
		<view class="example-body">
			<view v-for="item in items">
				<view class="uni-padding-wrap uni-common-mt" style="background-color:white" @click="goDetail(item.id)">
					<view class="uni-flex uni-row">
						<view class="text uni-flex" style="width: 200rpx;height: 220rpx;-webkit-justify-content: center;justify-content: center;-webkit-align-items: center;align-items: center;">
							<image :src="item.cover" style="width: 150rpx;height: 150rpx;"></image>
						</view>
						<view class="uni-flex uni-column" style="-webkit-flex: 1;flex: 1;-webkit-justify-content: space-between;justify-content: space-between;">
							<view class="text" style="font-size: 20px;  height: 120rpx;text-align: left;padding-left: 20rpx;padding-top: 10rpx;">
								{{item.name}}
							</view>
							<view class="uni-flex uni-row">
								<view class="text" style="-webkit-flex: 1;flex: 1;">收藏人数：{{item.collectionNumber}}</view>
								<view class="text" style="-webkit-flex: 1;flex: 1;">浏览人数：{{item.browseNumber}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
	export default {
		components: {
			uniNavBar
		},
		data() {
			return {
				search_value: 0,
				myFcous:true,

				// items:[{
				// 	url:'../../satic/plus.png',
				// 	name:'青椒肉丝',
				// 	numbers:'9',
				// 	satisfaction:"5",
				// },
				// {
				// 	url:'../../satic/plus.png',
				// 	name:'青椒肉丝',
				// 	numbers:'9',
				// 	satisfaction:"5",
				// }],
				items: [],
				exist: []
			}
		},
		onShow: function(option) { //option为object类型，会序列化上个页面传递的参数
			this.myFcous = false;
			this.exist = [];
			this.items = [];
			try {
			    const value = uni.getStorageSync('search_value');
			    if (value) {
			        console.log(value);
					this.search_value = value;
			    }
			} catch (e) {
			    // error
			}
			
			uni.request({
				url: 'https://pope.utools.club/getRecipesByName', //仅为示例，并非真实接口地址。
				data: {
					name: this.search_value,
				},
				method: 'POST',
				header: {
					'content-type': 'application/x-www-form-urlencoded',
				},
				success: (res) => {
					console.log(res.data);
					this.text = 'request success';
					if (res.data.code == 4) {
						for (var i = 0; i < res.data.data.length; i++) {
							var temp = {
								id: res.data.data[i].id,
								name: res.data.data[i].name,
								cover: res.data.data[i].cover,
								collectionNumber: res.data.data[i].collectionNumber,
								browseNumber: res.data.data[i].browseNumber
							};
							this.exist.push(res.data.data[i].id);
							this.items.push(temp);
						}
					} else if (res.data.code == -4) {
						//alert("查询失败")
					}
				},
				fail: () => {
					alert("网络在开小差，请重试");
				}
			});
		
			uni.request({
				url: 'https://pope.utools.club/getRecipesByMaterial', //仅为示例，并非真实接口地址。
				data: {
					material: this.search_value,
				},
				method: 'POST',
				header: {
					'content-type': 'application/x-www-form-urlencoded',
				},
				success: (res) => {
					console.log("搜索结果:" + res.data.data);
					this.text = 'request success';
					if (res.data.code == 5) {
						for (var i = 0; i < res.data.data.length; i++) {
							if (this.exist.indexOf(res.data.data[i].id) == -1) {
								var temp = {
									id: res.data.data[i].id,
									name: res.data.data[i].name,
									cover: res.data.data[i].cover,
									collectionNumber: res.data.data[i].collectionNumber,
									browseNumber: res.data.data[i].browseNumber
								};
								this.items.push(temp);
							}
						}
					} else if (res.data.code == -5) {
						//alert("查询失败")
					}
				},
				fail: () => {
					alert("网络在开小差，请重试");
				}
			});
		},
		methods: {
			goDetail(e) {
				uni.navigateTo({
					url: '../detail/detail?id=' + e
				})
				console.log(e)
			},
			toSearch_2() {
				
				uni.navigateTo({
					url: '../search/search',
					animationType: 'pop-in',
					animationDuration: 200
				});
			}
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

	.nav-bar{
		margin-top: 45px;
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
