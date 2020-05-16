<template>
	<view>
			 <view class="text">
				 <image :src="this.list.cover" style="height: 400upx;"></image>
				
				 		
			 </view>
<view style="text-align: center;margin-bottom: 10px;">
<text style="text-align: center;display: block;">{{this.list.name}}</text>
<text style="font-size: 12px;color: #9E9E9E">{{this.list.browseNumber}}浏览/{{this.list.collectionNumber}}收藏</text>
</view>
<view style="width: 100%;height:3px;margin: 0;background-color:#D9D9D9 ;"></view>
<!-- <view style="margin: 5px;margin-top: 10px;">
	 <text style="margin: 5px;margin-top: 10px;">分享</text>
	 <text style="display: block;margin: 5px;margin-top: 10px;font-size: 14px;">分享的内容.....</text>
 </view> -->

    <view class="uni-input" style="text-align: left;margin: 5px;padding: 5px;font-size: 14px;">作者：<span style="">{{this.list.author}}</span></span></view>
	<view class="uni-input" style="text-align: left;margin: 5px;padding: 5px;font-size: 14px;">种类：<span style="">{{this.list.type}}</span></span></view>
	<view class="uni-input" style="text-align: left;margin: 5px;padding: 5px;font-size: 14px;">时间：<span style="">{{this.list.time}}</span></span></view>
	<view class="uni-input" style="text-align: left;margin: 5px;padding: 5px;font-size: 14px;">难度：<span style="">{{this.list.difficulty}}级</span></span></view>
<view><text style="margin-left: 10px;">食材清单</text></view>
<view style="margin-bottom: 10px;" v-for="(item,key) in list.material" >
<view style="margin: 10px;text-align: center;display: block;">
	<text style="width: 50%;float: left;border-radius: 10px;font-size: 14px;">{{key}}</text>
	<text style="width: 50%;float: left;border-radius: 10px;font-size: 14px;">{{item}}</text>
	 </view>

	 
		 
		 
	</view>


	<view style="margin-top: 10px;margin-bottom: 10px;"><text style="margin-left: 10px;margin-top: 10px;">烹饪步骤</text>
	</view>
	
	<view v-for="item in list.howToDoVoList">
	<view>
	<view><text style="margin-left:10px">第{{item.order+1}}步</text></view>
	
	<view class="text">
	<image :src="item.mediaUrl" style="height: 400upx;"></image>
	</view>
	<text  class="uni-input"  style="width:90%;text-align:left;margin:auto; padding: 5px; border-radius: 5px;font-size: 14px;" confirm-type="done">{{item.description}}</text>
	</view>
	
	
	
	</view>
	
	
	</view>
	
</template>

<script>
	
	
	export default {
		
		data() {
			return {
				array:['10分钟左右','10~30分钟','30~60分钟','1小时以上'],
				index:0,
				array2:['初级','中级','高级'],
				index2:0,
				list:[]
			}
		},
		 onNavigationBarButtonTap(e) { //导航栏自定义按钮点击监听事件
		 console.log(e)
		 uni.switchTab({
		 	url:"../index/index",
			"open-type":"switchTab"
		 })
		 },
		 onLoad(e) {
		 	console.log(e.id);
			uni.request({
				url: 'http://pope.utools.club/getRecipeInfoById',
				data: {
					        Id: e.id
					    },
				method:'POST',
				header: {
					'content-type': 'application/x-www-form-urlencoded', 
				},
				success: (ret) => {
					console.log(ret.data)
					this.list=ret.data.data
				}
			});
		 },
		methods: {
			 bindPickerChange: function(e) {
			            console.log('picker发送选择改变，携带值为', e.target.value)
			            this.index = e.target.value
			        },
			bindPickerChange2: function(e) {
			           console.log('picker发送选择改变，携带值为', e.target.value)
			           this.index2 = e.target.value
			       },
			
			
			
		}
	}
</script>

<style>

.text{
	
	height:400upx;
}
.uni-input{
	text-align: center;
	
}
</style>
