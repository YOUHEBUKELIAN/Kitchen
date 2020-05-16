<template>
	<view>
		<image :src="pic" style="
	height: 150px;
	line-height: 150px;width: 100%; z-index: 3;">
		 <view  class="up" id="up"  @click="choosePicture()" style="position: absolute;top:0;z-index: 4;" >
			 <view class="text">
				 <image src="../../static/img/upload.png" style="width: 16px;height: 16px;"></image>
				 <text style="color: #A3A3A3;font-size: 15px;">上传封面</text>
				 		
			 </view>
		 </view>
		 </image>
 <input class="uni-input" id="name"  confirm-type="done" style="font-size: 18px;margin: 5px;" placeholder="给这道菜谱起个名" />
 
 <picker @change="bindPickerChange" :value="index" :range="array">
	<view class="uni-input" style="text-align: left;margin: 5px;padding: 5px;font-size: 14px;">时间：<span style="color: #4CD964;">{{array[index]}} ∨ </span></span></view>
</picker>
 <picker @change="bindPickerChange2" :value="index2" :range="array2">
	<view class="uni-input" style="text-align: left;margin: 5px;padding: 5px;font-size: 14px;">难度：<span style="color: #4CD964;">{{array2[index2]}} ∨ </span></span></view>
</picker>
<picker @change="bindPickerChange3" :value="index3" :range="array3">
	<view class="uni-input" style="text-align: left;margin: 5px;padding: 5px;font-size: 14px;">种类：<span style="color: #4CD964;">{{array3[index3]}} ∨ </span></span></view>
</picker>
<view><text style="margin-left: 10px;">添加食材</text><text style="float: right;color: #4CD964;font-size: 14px;margin-right: 10px;" @click="clearAll">清空</text></view>
<view style="margin: auto;">
<view style="margin: 10px;text-align: center;" v-for="item in mater" :key="item.id" id="materials">
	<view :id="item.oid">
	<input :id="item.oid" @input="getMaterial" type="text" style="width: 45%;float: left;background-color: #DBDBDB;border-radius: 10px;"placeholder="食材:如五花肉"/>
	<input :id="item.oid" @input="getCount" placeholder="用量:如50g" style="width: 45%;background-color: #DBDBDB;float: left;border-radius: 10px;"/>
	 <image :id="item.oid" src="../../static/img/delete.png" style="width: 16px;height: 16px;float: right;" @click="dele"></image>
	</view>
	

	 	 </view>
	
		 
		 
	</view>

<view style="margin:20px;text-align: center;">
<button type="primary" @click="add()" style="margin: auto;border-radius: 30px;width:80%;height:30px;text-align: center;line-height: 30px;font-size: 14px;display: block;" plain="true">再添加一种食材</button>	
	</view>
	<view><text style="margin-left: 10px;">烹饪步骤</text><text style="float: right;color: #4CD964;font-size: 14px;margin-right: 10px;">批量添加
	</text></view>
	<view v-for="item in foot" :key="item.id">
		<view :id="item.id">
	<view><text style="margin-left:10px">第{{item.id+1}}步</text></view>
	<image :src="uploadPics[item.id]" style="margin-top: 10px;margin-bottom: 10px;
	height: 150px;
	line-height: 150px;width: 100%; position: relative; z-index: 8;">
		 <view  class="upfoot" :id="'hhh'+item.id"  @click="uploadPic" style="position: relative;margin-top: -165px; z-index: 9;" >
			 <view class="text">
				 <image src="../../static/img/upload.png" style="width: 16px;height: 16px;"></image>
				 <text style="color: #A3A3A3;font-size: 15px;">上传封面</text>
				 		
			 </view>
		 </view>
		 </image>
	<textarea  class="uni-input" :id="item.id" @input="getFoot" style="background-color: #DBDBDB;width:90%;text-align:left;margin:auto; padding: 5px; border-radius: 5px;font-size: 14px;" confirm-type="done" placeholder="步骤详细描述" auto-height />
	
	
	</view>
	</view>
	<view style="margin:20px;text-align: center;margin-bottom: 40px;">
	<button type="primary" @click="addFoot" style="margin: auto;border-radius: 30px;width:80%;height:30px;text-align: center;line-height: 30px;font-size: 14px;display: block;" plain="true">增加一步</button>	
		</view>
		
	<view style="margin:0px;text-align: center;position: fixed;bottom: 50px;width: 100%;background-color: white;">
	<button type="primary" @click="submit" style="margin: auto;border-radius: 30px;width:80%;height:30px;text-align: center;line-height: 30px;font-size: 14px;display: block;" plain="true">发布菜谱</button>	
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
				array3:['炸','炒','烤','焖','蒸','煎','煮'],
				index3:0,
				pic:'',
				picfile:{},
				mater:[{
					id:0,
					oid:0
				},{id:1,
				oid:1}],
				i:1,
				j:0,
				foods:[],
				foot:[{
					id:0,
				}],
				uploadPics:[],
				uploadPicsFile:[],
				counts:[],
				express:[]
			}
		},
		 onNavigationBarButtonTap(e) { //导航栏自定义按钮点击监听事件
		 console.log(e)
		 uni.switchTab({
		 	url:"../index/index",
			"open-type":"switchTab"
		 })
		 },
		 onLoad() {
		 	
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
			bindPickerChange3: function(e) {
			           console.log('picker发送选择改变，携带值为', e.target.value)
			           this.index3 = e.target.value
					   
			       },
			add:function(e){
				this.i++
				var d={oid:this.i,
				id:this.i}
			this.mater.push(d)
			console.log(this.mater)
			
			for(var i=0;i<this.mater.length;i++){
				console.log(this.mater[i].id)
				console.log(this.mater[i].oid)
				//console.log(document.getElementById(this.mater[i].id).nodeValue)
			}
			},
			addFoot:function(e){
				this.j=this.j+1
				var d={
				id:this.j}
			this.foot.push(d)
			console.log(this.foot)
			},
			getFoot:function(e){
				this.express[e.currentTarget.id]=e.target.value
				console.log(e.currentTarget.id)
				console.log(this.express)
				console.log(this.uploadPics)
				
			},
			
			getMaterial:function(e){
				this.foods[e.target.id]=e.target.value
				console.log(e.target.id)
				console.log(this.foods)
			},
			getCount:function(e){
				this.counts[e.target.id]=e.target.value
				console.log(e.target.id)
				console.log(this.counts)
			},
			dele:function(e){
				console.log(e.target.id)
				this.i=this.i-1
				var i=0
				for(;i<this.mater.length;i++){
					if(this.mater[i].id==e.target.id)
					{
						this.mater.splice(i,1)
						this.foods.splice(i,1)
						this.counts.splice(i,1)
						break
					}
				}
				for(;i<this.mater.length;i++){
					this.mater[i].oid=this.mater[i].oid-1
				 }
				console.log(this.mater)
			},
			clearAll:function(e){
				this.mater=[]
				this.foods=[]
				this.counts=[]
				this.i=-1
				
			},
			choosePicture:function(e){
				   	console.log("hhh")
					var that =this
					console.log(that.pic)
					uni.chooseImage({
				   	                    // count:  允许上传的照片数量
				   	                    count:1,
				   	                    // sizeType:  original 原图，compressed 压缩图，默认二者都有
				   	                    sizeType: "original",
				   	                    success(res){
				   	                        console.log(res)
											that.pic=res.tempFilePaths[0]
											that.picfile=new Object()
											that.picfile.name=res.tempFiles[0].name+"hh"
											that.picfile.file=res.tempFiles[0]
											that.picfile.uri=res.tempFilePaths[0]
											
											var a=document.getElementById('up')
											a.style.zIndex=2
				   	                        // uni.previewImage({
				   	                        //     // 对选中的图片进行预览
				   	                        //     urls: res.tempFilePaths,
				   	                        //     // urls:['','']  图片的地址 是数组形式
				   	                        // })
				   	                    } 
				   	                });
									console.log(that.pic)
				   	
				   },
		uploadPic:function(e){
			   	console.log("hhh")
				var that =this
				console.log(that.uploadPics)
				console.log(e.currentTarget.id)
				uni.chooseImage({
			   	                    // count:  允许上传的照片数量
			   	                    count:1,
			   	                    //sizeType:  original 原图，compressed 压缩图，默认二者都有
			   	                    sizeType: "original",
			   	                    success(res){
			   	                        console.log(res)
										
										var mun=parseInt(e.currentTarget.id.replace(/[^0-9]/ig,""));
										console.log(mun)
										that.uploadPics[mun]=res.tempFilePaths[0]
										let obj = new Object();  
										          obj.name = res.tempFiles[0].name+mun;  
												  obj.file=res.tempFiles[0];
										          obj.uri = res.tempFilePaths[0];  
										that.uploadPicsFile[mun]=obj
										var a=document.getElementById(e.currentTarget.id)
										a.style.zIndex=0
									
										console.log(a)
										var resdata=JSON.parse(JSON.stringify(that.uploadPics))
										console.log(resdata[0])
										console.log(resdata)
										that.uploadPics=resdata
										
			   	                        // uni.previewImage({
			   	                        //     // 对选中的图片进行预览
			   	                        //     urls: res.tempFilePaths,
			   	                        //     // urls:['','']  图片的地址 是数组形式
			   	                        // })
			   	                    } 
			   	                });
								
								console.log(that.uploadPics)
			   	
			   },
		submit:function(e){
			console.log(this.pic)
			console.log(this.uploadPics)
			// this.uploadPicsFile.splice(0,0,this.picfile);
			console.log(this.express)
			
			var mas=[]
			for(var i=0;i<this.foods.length;i++)
			{
			var ma={}
			ma[this.foods[i]]=this.counts[i]
			mas.push(ma)
			}
			console.log(mas)
			console.log(document.getElementById('name').value)
			
			console.log(this.array[this.index])
			console.log(this.array2[this.index2])
			
			// uni.request({
			// 	url: 'http://pope.utools.club/uploadRecipe',
			// 	method:'POST',
			// 	dataType:'formData',
			// 	contentType: 'multipart/form-data',
			// 	header: {
			// 		'content-type': 'false',
			// 		// 'enctype':'multipart/form-data'
			// 		'dataType': "formData",
			// 	},
			// 	formdata: {
			// 		        'name':document.getElementById('name').value,
			// 				'author':"刘盲",
			// 				'time':this.array[this.index],
			// 				'material':mas,
			// 				'difficulty':this.array2[this.index2],
			// 				'type':this.array3[this.index3],
			// 				'description':this.express,
			// 				'files':this.uploadPicsFile
			// 		    },
			uni.uploadFile({
												url: 'http://pope.utools.club/uploadRecipe',
												filePath:this.pic,
												name: 'cover',
												fileType: 'image',
												formData:{
													name:document.getElementById('name').value,
													author:"你是个傻瓜",
													time:this.array[this.index],
													material:mas,
													difficulty:this.array2[this.index2],
													type:this.array3[this.index3],
													description:this.express,
												},
												success: (ret) => {
													console.log(ret)
													var chengle=true;
													var id=JSON.parse(ret.data);
													var that=this
													console.log(id+" "+ret.data)
													for(var i=0;i<that.uploadPics.length;i++)
													{
													uni.uploadFile({
																						url: 'http://pope.utools.club/uploadHowToDo',
																						filePath:that.uploadPics[i],
																						name: 'file',
																						fileType: 'image',
																						formData:{
																							Id:parseInt(id.data),
																							description:that.express[i],
																							order:parseInt(i+1)
																						},
																						success: (ret) => {
																							console.log(ret.data)
																							if(JSON.parse(ret.data).data==null)
																							{
																								chengle=false;
																							}
																						}
																					});
																					}
												if(chengle==true)
												{
														uni.showToast({
															title: '上传成功',
															icon: 'none',
														});
														console.log(ret)
														
														uni.switchTab({
															url:"../upload1/upload1",
															"open-type":"switchTab"
														})
												}else{
													uni.showToast({
														title: '上传失败',
														icon: 'none',
													});
													uni.redirectTo({
														url:"../upload1/upload1"
													})
												}
												}
											});
			//  uni.uploadFile({
			//                         url:'http://pope.utools.club/uploadRecipe',     // 后端api接口
			//                         //fileType:'image',
			// 						//contentType: 'multipart/form-data',
			// 						files: this.uploadPicsFile, // uni.chooseImage函数调用后获取的本地文件路劲
			//                         name:'files',     //后端通过'file'获取上传的文件对象(字段)
			//                         formData: {
			//                              // openid:_self.openid,  //剩下的字段
			// 							 name:document.getElementById('name').value,
			// 							 author:"刘盲",
			// 							 time:this.array[this.index],
			// 							 material:mas,
			// 							 difficulty:this.array2[this.index2],
			// 							 type:this.array3[this.index3],
			// 							 description:this.express,
										 				
			//                          }, 
			//                         //header:{"Content-Type": "multipart/form-data"},
			// 	success: (ret) => {
					
			// 	uni.showToast({
			// 		title: '上传成功',
			// 		icon: 'none',
			// 	});
			// 	console.log(ret)
			// 	uni.switchTab({
			// 		url:"../index/index",
			// 		"open-type":"switchTab"
			// 	})
			// 	}
			// });
		}
		
			
			
		}
	}
</script>

<style>
.up{
	background-color: #DBDBDB;
	
	height: 150px;
	width: 100%;
	line-height: 150px;

}
.upfoot{
	background-color: #DBDBDB;
	margin-top: 10px;
	margin-bottom: 10px;
	height: 150px;
	width: 100%;
	line-height: 150px;
}
.text{
	text-align: center;
}
.uni-input{
	text-align: center;
	
}
</style>
