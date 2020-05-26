<template>
	<view class="page-section-spacing">
		
		<swiper class="swiper" 
			indicator-dots="true" 
			autoplay="true" 
			interval="5000" 
			duration="1500"	>
			<swiper-item v-for="(item , index) in homeSlide" :key="index">
				<image :src="item.cover" mode="" @click="goDetail(item.id)"></image>
			</swiper-item>
		</swiper>
	<view class="grid">
			<view class="grid-c-06" v-for="item in lists" :key="item.id">
				<view class="panel" @click="goDetail(item.id)">
					<image class="card-img card-list2-img" :src="item.cover"></image>
					
					<view class="card-bottm row">
						<view class="car-title-view row">
							<text class="card-title card-list2-title">{{item.name}}</text>
							<text class="card-title card-list2-title">{{item.browseNumber}}浏览 {{item.collectionNumber}}收藏</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<text class="loadMore">{{this.text}}</text>
	</view>
	</view>
	
	
</template>

<script>
	
	export default {
		data() {
			return {
				refreshing: false,
				number:0,
				lists: [],
				text:"加载中...",
				fetchPageNum: 1,
				homeSlide: []
					, // 定义值接收轮播图数据{src:"../../static/img/tab_01_c.png"},
				//{src:"../../static/logo.png"}
			}
		},
		//在 onLoad 生命周期中 获取服务器接口，返回成功后将结果绑定到 布局中
		// getHomeSlideFunc() {
		// 	var _self = this;
		// 	uni.request({
		// 		url: 'http://appblog.inacorner.top/wp-content/themes/wpApp/api/homeSlide.php',
		// 		success: (res) => {
		// 			_self.homeSlide = res.data.post;
		// 		}
		// 	});
		//},
		
	onLoad() {
			this.getData();
			uni.request({
				url: 'http://pope.utools.club/getCarouselRecipe',
				method:'GET',
				header: {
					'content-type': 'application/x-www-form-urlencoded', 
				},
				success: (ret) => {
					console.log(ret)
					this.homeSlide=ret.data.data
					console.log("hhh")
					}
					})
		},
	onNavigationBarButtonTap(e) {
	    console.log("success")
		uni.navigateTo({
			url:"../upload1/upload1"
		})
	},
		onPullDownRefresh() {
			console.log('下拉刷新hhhh');
			this.refreshing = true;
			this.getData();
		},
		onReachBottom() {
			this.getData();
		},
		methods: {
			swiperClick(item){
			        console.log("点banner的link=", item);  
			    },
			getData() {
				uni.request({
					url: 'http://pope.utools.club/getMainRecipesByPageNumber',
					data: {
						        pageNumber: this.number
						    },
					method:'POST',
					header: {
						'content-type': 'application/x-www-form-urlencoded', 
					},
					success: (ret) => {
						console.log(ret)
						if (ret.statusCode !== 200) {
							console.log('请求失败:', ret)
						} else {
							if (ret.data.code==-1) {
								uni.showToast({
									title: '已经最新',
									icon: 'none',
								});
								this.refreshing = false;
								uni.stopPullDownRefresh();
								this.text='到底啦~\(≧▽≦)/~'
								return;
							}
							let list = ret.data.data;
								// data = ret.data;
							// for (let i = 0, length = data.length; i < length; i++) {
							// 	var item = data[i];
							// 	item.guid = this.newGuid() + item.id
							// 	list.push(item);
							// }
							console.log('得到list', list);
							// this.lists=this.lists.concat(list);
							// console.log(this.lists);
							// this.number=this.number+1;
							if (this.refreshing) {
								this.refreshing = false;
								uni.stopPullDownRefresh()
								this.lists = list;
								this.number = 1;
							} else {
								if(list!=null){
								this.lists = this.lists.concat(list);
								this.number += 1;}
							}
							console.log(this.lists);
						}
					}
				});
			},
			// newGuid() {
			// 	let s4 = function() {
			// 		return (65536 * (1 + Math.random()) | 0).toString(16).substring(1);
			// 	}
			// 	return (s4() + s4() + "-" + s4() + "-4" + s4().substr(0, 3) + "-" + s4() + "-" + s4() + s4() + s4()).toUpperCase();
			// },
			goDetail(e) {
				uni.navigateTo({
					url: '../detail/detail?id='+e 
				})
				console.log(e)
			},
			share(e) {
				if (this.providerList.length === 0) {
					uni.showModal({
						title: '当前环境无分享渠道!',
						showCancel: false
					})
					return;
				}
				let itemList = this.providerList.map(function(value) {
					return value.name
				})
				uni.showActionSheet({
					itemList: itemList,
					success: (res) => {
						uni.share({
							provider: this.providerList[res.tapIndex].id,
							scene: this.providerList[res.tapIndex].type && this.providerList[res.tapIndex].type === 'WXSenceTimeline' ?
								'WXSenceTimeline' : 'WXSceneSession',
							type: 0,
							title: 'uni-app模版',
							summary: e.title,
							imageUrl: e.img_src,
							href: 'https://uniapp.dcloud.io',
							success: (res) => {
								console.log('success:' + JSON.stringify(res));
							},
							fail: (e) => {
								uni.showModal({
									content: e.errMsg,
									showCancel: false
								})
							}
						});
					}
				})
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.swiper{
		height: 400upx;
	}
	
	// swiper-item 里面的图片高度
	swiper-item image{
		width: 100%;
		height: 400upx;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	@font-face {
		font-family: texticons;
		font-weight: normal;
		font-style: normal;
		src: url('https://at.alicdn.com/t/font_702773_g9f89om4v3j.ttf') format('truetype');
	}
	
	.inde {
		flex: 1;
		width: 750upx;
		min-height: 100vh;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	
	.row {
		flex-direction: row;
	}
	
	.column {
		flex-direction: column;
	}
	
	.card {
		position: relative;
		width: 710upx;
		margin: 20upx 20upx 20upx 20upx;
		border-radius: 10upx;
		overflow: hidden;
		flex-direction: column;
		background-color: #FFFFFF;
	}
	
	.card-img {
		width: 710upx;
		height: 1065upx;
	}
	
	.card-num {
		color: #FFFFFF;
		font-size: 13px;
		text-align: center;
	}
	
	.card-num-view {
		background-color: #FF80AB;
		line-height: 1;
		display: inline-block;
		padding: 3px 6px;
		color: #FFFFFF;
		font-size: 12px;
		text-align: center;
		justify-content: center;
		align-items: center;
		border-radius: 15px;
		position: absolute;
		top: 20upx;
		right: 20upx;
	}
	
	.card-bottm {
		justify-content: center;
		align-items: center;
	}
	
	.card-share-view {
		justify-content: center;
		align-items: center;
		padding: 14upx 0;
		color: #FF80AB;
		margin: 20upx 20upx 20upx;
		font-size: 30upx;
		font-family: texticons;
	}
	
	.card-share-view:before {
		content: '\e62d';
	}
	
	.car-title-view {
		flex: 1;
		padding: 14upx 0upx 14upx 20upx;
	}
	
	.card-title {
		flex: 1;
		font-size: 30upx;
		text-align: left;
		color: #555555;
		text-overflow: ellipsis;
		lines: 2;
		display: -webkit-box;
		white-space: normal;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
		overflow: hidden;
	}
	
	
	
	
	
	
	
	
	
	/*双列表*/
	
	.card-list2 {
		width: 345upx;
		margin: 20upx 0 20upx 20upx;
	}
	
	.card-list2-img {
		width: 345upx;
		height: 250upx;
	}
	
	.card-list2-num-view {
		transform: scale(0.8);
		transform-origin: right;
	}
	
	.card-list2-num {
		font-size: 22upx;
	}
	
	.card-list2-title {
		font-size: 20upx;
	}
	
	
	.loadMore {
		font-size: 30upx;
		color: #555;
		margin-bottom: 20upx;
	}
	
	
	
	
	
	
	/* 分类页面 */
	
	.tags {
		background: #fff;
		flex: 1;
		width: 710upx;
		margin: 20upx;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: flex-start;
		align-content: flex-start;
	}
	
	view.tag,
	.tag {
		display: block;
		width: 177.5upx;
		height: 180upx;
		padding: 24upx;
		line-height: 1;
		box-sizing: border-box;
		text-align: center;
	}
	
	.tag-img {
		border-radius: 60upx;
		width: 120upx;
		height: 120upx;
	}
	
	.tag-text {
		font-size: 28upx;
		color: #555555;
	}
	
	
	
	
	
	/* 个人中心 */
	
	.center {
		flex-direction: column;
		min-height: 100vh;
	}
	
	.logo {
		width: 750upx;
		height: 240upx;
		padding: 20upx;
		box-sizing: border-box;
		background-color: #FF80AB;
		flex-direction: row;
		align-items: center;
	}
	
	.logo-hover {
		opacity: 0.8;
	}
	
	.logo-img {
		width: 150upx;
		height: 150upx;
		border-radius: 150upx;
	}
	
	.logo-title {
		height: 150upx;
		flex: 1;
		align-items: center;
		justify-content: space-between;
		flex-direction: row;
		margin-left: 20upx;
	}
	
	.uer-name {
		height: 60upx;
		line-height: 60upx;
		font-size: 38upx;
		color: #FFFFFF;
	}
	
	.go-login.navigat-arrow {
		font-size: 38upx;
		color: #FFFFFF;
	}
	
	.login-title {
		height: 150upx;
		align-items: self-start;
		justify-content: center;
		flex-direction: column;
		margin-left: 20upx;
	}
	
	.center-list {
		background-color: #FFFFFF;
		margin-top: 20upx;
		width: 750upx;
		flex-direction: column;
	}
	
	.center-list-item {
		height: 90upx;
		width: 750upx;
		box-sizing: border-box;
		flex-direction: row;
		padding: 0upx 20upx;
	}
	
	.border-bottom {
		border-bottom-width: 1upx;
		border-color: #c8c7cc;
		border-bottom-style: solid;
	}
	
	.list-icon {
		width: 40upx;
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #FF80AB;
		text-align: center;
		font-family: texticons;
		margin-right: 20upx;
	}
	
	.list-text {
		height: 90upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		flex: 1;
		text-align: left;
	}
	
	.navigat-arrow {
		height: 90upx;
		width: 40upx;
		line-height: 90upx;
		font-size: 34upx;
		color: #555;
		text-align: right;
		font-family: texticons;
	}
	
	
	
	
	/* 登录 */
	
	.loginView {
		display: flex;
		flex: 1;
		flex-direction: column;
		width: 750upx;
		min-height: 100vh;
		padding-top: 30upx;
	}
	
	.input-view {
		border-bottom-style: solid;
		border-bottom-width: 1upx;
		border-bottom-color: #ddd;
		background-color: #fff;
		flex-direction: row;
		width: 750upx;
		padding: 20upx 20upx;
		box-sizing: border-box;
	}
	
	.label-view {
		width: 100upx;
		height: 60upx;
		align-items: center;
		margin-right: 30upx;
	}
	
	.label {
		flex: 1;
		line-height: 60upx;
		font-size: 38upx;
		color: #555;
		text-align: left;
	}
	
	.input {
		flex: 1;
		height: 60upx;
		font-size: 38upx;
		align-items: center;
	}
	
	.button-view {
		width: 750upx;
		margin-top: 50upx;
		padding: 0 20upx;
		box-sizing: border-box;
		flex-direction: column;
	}
	
	button {
		width: 710upx;
		height: 80upx;
		line-height: 80upx;
		text-align: center;
		font-size: 38upx;
	}
	
	button.login {
		background-color: #FF80AB;
		color: #fff;
	}
	
	button.register {
		margin-top: 30upx;
		color: #FF80AB;
		background-color: #fff;
		border-color: #FF80AB;
		border-width: 2upx;
	}
	
	.register.hover,
	.login.hover {
		opacity: 0.6;
	}
	
	
	
	/* 详情页面 */
	
	.detail-btn-view {
		width: 750upx;
		position: fixed;
		bottom: 75upx;
		font-size: 28upx;
		justify-content: space-between;
	}
	
	.detail-btn-view view {
		opacity: 0.75;
		height: 80upx;
		border-radius: 80upx;
		width: 80upx;
		justify-content: center;
		align-items: center;
		line-height: 80upx;
		margin: 0 30upx;
		font-family: texticons;
		background: #eee;
		color: #555;
	}
	
	.detail-btn-view view text {
		margin-left: 10upx;
	}
	
	.detail-btn-view .download:before {
		content: '\e617';
	}
	
	.detail-btn-view .collect:before {
		content: '\e68d';
	}
	
	.detail-btn-view .setting {
		flex: 1;
	}
	
	
	.grid {
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		padding-left: 5px;
		padding-right: 5px;
		box-sizing: border-box;
	}
	
	.grid-c-01,
	.grid-c-02,
	.grid-c-03,
	.grid-c-04,
	.grid-c-05,
	.grid-c-06,
	.grid-c-07,
	.grid-c-08,
	.grid-c-09,
	.grid-c-10,
	.grid-c-11,
	.grid-c-12 {
		position: relative;
		width: 100%;
		padding-left: 5px;
		padding-right: 5px;
		box-sizing: border-box;
	}
	
	.grid-c-auto {
		flex: 0 0 auto;
		width: auto;
		max-width: none;
	}
	
	.grid-c-fill {
		flex-basis: 0;
		flex: 1;
		flex-grow: 1;
		max-width: 100%;
	}
	
	.grid-c-none {
		flex: none;
	}
	
	.grid-c-01 {
		flex: 0 0 8.333333%;
		max-width: 8.333333%;
	}
	
	.grid-c-02 {
		flex: 0 0 16.666667%;
		max-width: 16.666667%;
	}
	
	.grid-c-03 {
		flex: 0 0 25%;
		max-width: 25%;
	}
	
	.grid-c-04 {
		flex: 0 0 33.333333%;
		max-width: 33.333333%;
	}
	
	.grid-c-05 {
		flex: 0 0 41.666667%;
		max-width: 41.666667%;
	}
	
	.grid-c-06 {
		flex: 0 0 50%;
		max-width: 50%;
	}
	
	.grid-c-07 {
		flex: 0 0 58.333333%;
		max-width: 58.333333%;
	}
	
	.grid-c-08 {
		flex: 0 0 66.666667%;
		max-width: 66.666667%;
	}
	
	.grid-c-09 {
		flex: 0 0 75%;
		max-width: 75%;
	}
	
	.grid-c-10 {
		flex: 0 0 83.333333%;
		max-width: 83.333333%;
	}
	
	.grid-c-11 {
		flex: 0 0 91.666667%;
		max-width: 91.666667%;
	}
	
	.grid-c-12 {
		flex: 0 0 100%;
		max-width: 100%;
	}
	
	.panel {
		position: relative;
		border-radius: 10upx;
		overflow: hidden;
		flex-direction: column;
		background-color: #FFFFFF;
		margin-bottom: 10px;
	}
	
</style>
