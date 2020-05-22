 <template>	
	<view>1231321</view>
	
</template>

<script>
	import adTabbar from '@/components/andy-ADTabbar/andy-ADTabbar.vue';
	import adTabbarItem from '@/components/andy-ADTabbar/andy-ADTabbarItem.vue';
	
	export default {
		components: {
		    adTabbar,
			adTabbarItem
		},
		data() {
			return {
				curPage: 'page1',
			}
		},
		onLoad() {
		},
		methods: {
			// 导航栏切换
			navClick: function(e) {
				console.log(this.curPage);
				console.log(e.currentTarget.dataset.cur);
				this.curPage = e.currentTarget.dataset.cur;
				
			},
			// 中间按钮点击
			middleClick: function(e) {
				console.log("mid clicked");
				uni.navigateTo({
				    url: '../mainPages/page5',
				    animationType: 'slide-in-bottom',
				    animationDuration: 200
				});
			},
		}
	}
</script>

<style>
	@import '../../common/uni-nvue.css';
	.middleItem {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		
		font-size: 22upx;
	}
	.buttonView {
		width: 100upx;
		height: 100upx;
		border-radius: 50upx;
		margin-top: -60upx;
		background-color: #349E40;
		margin-bottom: 10upx;
		
		color: #FFFFFF;
		line-height: 100upx;
		text-align: center;
		font-size: 50upx;
	}
</style>
