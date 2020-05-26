<template>
	<view class="center">
		<view class="center_top">
			<view class="mask"></view>
		</view>
		<view class="center_box_bg">
			<view class="profily">
				<view class="base">
					<view class="profily_header">

					</view>
					<text>{{name}}</text>
					<image src="../../static/fumou-center-template/setting.png" mode="" @click="goSetting()"></image>
				</view>
				<text style="font-size: 15px;">{{time}}加入</text>
				<view class="block">
					<text class="block">{{selfIntro}}</text>
				</view>
				<view class="block" style="margin-top: 7px;">
					<text class="flex_margin">{{focus}}</text>
					<text class="flex_margin">{{fans}}</text>
					<text class="block">关注 粉丝</text>
				</view>
			</view>

			<view class="baiban">

			</view>
			<view class="baiban">

			</view>

			<view class="order_status">
				<view class="status" v-for="item in status" @click="goDetail(item)">
					<image class="icon" :src="item.url" mode="aspectFill"></image>
					<text>{{item.name}}</text>
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				nickname: "blue",
				name:"",

				items: [{
					url: '../../static/tabar/heart.png',
					description: '12315544488979844',
					author: 'qiang',
				}],

				status: [{
						key: 1,
						name: '我的菜谱',
						url: '../../static/personal_page/my.png',
						pageurl: "./myRecipe"
					},
					{
						key: 2,
						name: '我的草稿箱',
						url: '../../static/personal_page/caogao.png',
						pageurl: "./myRecipe"
					},
					{
						key: 3,
						name: '我的收藏',
						url: '../../static/personal_page/heart.png',
						pageurl: "./myRecipe"
					},
					{
						key: 4,
						name: '浏览历史',
						url: '../../static/personal_page/history.png',
						pageurl: "./myRecord"
					}
				],

				menus: [{
						name: '我的收藏',
						icon: '../../static/fumou-center-template/5.png',
						key: 1,
					},
					{
						name: '地址管理',
						icon: '../../static/fumou-center-template/6.png',
						key: 2,
					},
					{
						name: '尺码对照表',
						icon: '../../static/fumou-center-template/7.png',
						key: 3,
					},
					{
						name: '帮助中心',
						icon: '../../static/fumou-center-template/8.png',
						key: 4,
					},
					{
						name: '意见反馈',
						icon: '../../static/fumou-center-template/9.png',
						key: 5,
					},
					{
						name: '关于我们',
						icon: '../../static/fumou-center-template/10.png',
						key: 6,
					}

				],

				userId: 100,
				time: "2019-2-13",
				selfIntro: "添加个人简介,让厨友更加了解你",
				focus: 0,
				fans: 0,
			};
		},
		onShow: function(option) {
			try{
			    const value = uni.getStorageSync('nickName');
			    if(value){
			        console.log(value)
					this.nickname = value;
					this.name = value;
			    }
			}catch(e){
			   console.log("this.value")
			};
		},
		methods: {
			goDetail(e) {
				var url = "" + e.pageurl + "?nickname=" + this.nickname;
				console.log(url);
				uni.navigateTo({
					url: url
				})
				console.log(e)
			},
			goSetting() {
				uni.navigateTo({
					url: '../certification/certification'
				})
			},
		},
		computed: {

		}
	}
</script>

<style lang="scss">
	page {
		height: 100%;
	}

	.profily,
	.profily_header {
		border-radius: 8px;
	}

	.center {
		height: 100%;

		&_top {
			height: 18%;
			background: url('../../static/fumou-center-template/header.jpg') no-repeat 0% 50%;
			background-size: cover;

			// background: #E6E6E6;
			.mask {
				background: rgba(0, 0, 0, .4);
				height: 100%;
			}
		}

		&_box_bg {
			background: #F9F9F9;
			position: relative;

			.profily {
				position: absolute;
				width: 90%;
				// border:1px solid #F7F7F7;
				margin: 0 auto;
				top: -100upx;
				left: 5%;
				background: #FEFEFE;
				padding: 35upx;
				box-sizing: border-box;
				box-shadow: 0px 2px 5px #EDEDED;
			}
		}
	}

	.base {
		display: flex;
		align-items: center;
		border-bottom: 2px solid #F6F6F6;
		padding-bottom: 35upx;
		position: relative;

		.profily_header {
			height: 120upx;
			width: 120upx;
			background-image: url('../../static/fumou-center-template/header.jpg');
			background-size: 100%;
		}

		text {
			margin-left: 20px;
			font-size: 30upx;
		}

		image {
			position: absolute;
			height: 40upx;
			width: 40upx;
			right: 0px;
			top: 0px;
		}
	}

	.order_status {
		display: flex;
		justify-content: space-between;
		margin-top: 35upx;
		padding-top: 100px;

		.status {
			width: 140upx;
			font-size: 24upx;
			text-align: center;
			letter-spacing: .5px;
			display: flex;
			flex-direction: column;

			.icon {
				width: 50upx;
				height: 50upx;
				margin: 0 auto;
				margin-bottom: 5px;

			}
		}
	}

	.baiban {
		background: #FEFEFE;
		height: 90upx;
	}

	.center_menu {
		width: 100%;
		display: inline-block;

		.menu_item {
			font-size: 28upx;
			letter-spacing: 1px;
			padding: 14px 5%;
			background: #FEFEFE;
			overflow: hidden;
			box-sizing: border-box;
			display: flex;
			align-items: center;
			position: relative;
			border-bottom: 1px solid #EFEFEF;

			&:hover {
				background: #F6F6F6 !important;
			}

			&::after {
				content: '';
				width: 30upx;
				height: 30upx;
				position: absolute;
				right: 5%;
				background: url('../../static/fumou-center-template/right.png') no-repeat;
				background-size: 100%;
			}

			text:nth-of-type(1) {
				margin-left: 10px;
			}

			image {
				width: 40upx;
				height: 40upx;
			}

			&:nth-of-type(4) {
				margin-top: 10px;
			}
		}
	}

	.block {
		display: block;
		font-size: 15px;
	}

	.flex_margin {
		font-size: 15px;
		margin-right: 9%;
		font-weight: bold;
	}
</style>
