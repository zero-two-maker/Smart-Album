<template>
	<view>
		<!-- 顶部导航栏 -->
		<uni-nav-bar :class="[{nocheck:operation}]" fixed height="100px" shadow>
		    <view :style="{'margin-top':barHeight+'px'}">
				<uni-search-bar placeholder="请输入关键字" :radius="100" cancelButton="none" style="width: 220px; height: 30px;"></uni-search-bar>
			</view>
		    <view :style="{'margin-top':barHeight+'px'}" slot="left">
				<uni-icons type="person" size="25px"></uni-icons>
			</view>
		    <view :style="{'margin-top':barHeight+'px'}" slot="right">
				<uni-icons type="list" size="25px"></uni-icons>
			</view>
		</uni-nav-bar>
		<uni-nav-bar :class="[{nocheck:!operation}]" fixed height="100px" shadow>
		    <view :style="[{'margin-top':(barHeight+7)+'px'}]">
				<view style="width: 220px;">
				    <uni-title type="h2" title="请选择" align="center"></uni-title>
				</view>
			</view>
		    <view :style="{'margin-top':barHeight+'px'}" slot="left">
				<uni-icons type="closeempty" size="25" @click="closelongtap()"></uni-icons>
			</view>
		    <view :style="{'margin-top':barHeight+'px'}" slot="right">
			</view>
		</uni-nav-bar>
		<!-- <view class="content">
			<button  @click="apiClick()">测试跨域</button>
			<button  @click="searchClick()">测试查询节点信息</button>
			<button  @click="gotoyear()">测试跳转</button>
		</view> -->
		<view class="showview" style="margin-left: 0;margin-right: 0;" @touchmove="start" @touchend="end" >
			<checkbox-group name="imgs" @change="checkboxChange">
			<view  v-for="items in allimglist" style="width: 100%;">
				<uni-title :class="getyear(items.time)" type="h3" :title="gettime(items.time)" align="left" style="width: 400px;height: 30px; margin-left: 15px; margin-bottom: 10px;"></uni-title>
				<view  class="rows" v-for="(item,index) in items.imglist" style="width: 100%;">
					<view :class="['box-list','img'+item.id]">
						<image mode='aspectFill' style="width: 127px;height: 127px;" :src="httpurl+item.img" @click="showdetail(item.index)" @longtap="imglongtap()"></image>
						<checkbox :class="[{check:operation},{nocheck:!operation}]" :checked="item.checked" :value="item.id" />
					</view>
				</view>
				<br>
				<br>
				<br>
				<br>
				<br>
				<br>
			</view>
			</checkbox-group>
		</view>
		<!-- <view :class="[{bottomview:operation},{nocheck:!operation}]">
			<view class="operation_btn">
				<uni-icons  type="upload" size="20" color="#007BFF"></uni-icons>
				<label>分享</label>
			</view>
		</view> -->
		<uni-tag :class="[{nocheck:!slip}]" text="2017年" circle="true" :style="{position:'fixed', top:yearsposition[0]+'px',left:'280px'}" @click="gotoyear(yeartop[0])" customStyle="background-color: #f9f9f9; color:#000000; font-weight:bold; border: none;"></uni-tag>
		<uni-tag :class="[{nocheck:!slip}]" text="2018年" circle="true" :style="{position:'fixed', top:yearsposition[1]+'px',left:'280px'}" @click="gotoyear(yeartop[1])" customStyle="background-color: #f9f9f9; color:#000000; font-weight:bold; border: none;"></uni-tag>
		<uni-tag :class="[{nocheck:!slip}]" text="2019年" circle="true" :style="{position:'fixed', top:yearsposition[2]+'px',left:'280px'}" @click="gotoyear(yeartop[2])" customStyle="background-color: #f9f9f9; color:#000000; font-weight:bold; border: none;"></uni-tag>
		<uni-tag :class="[{nocheck:!slip}]" text="2020年" circle="true" :style="{position:'fixed', top:yearsposition[3]+'px',left:'280px'}" @click="gotoyear(yeartop[3])" customStyle="background-color: #f9f9f9; color:#000000; font-weight:bold; border: none;"></uni-tag>
		<uni-tag :class="[{nocheck:!slip}]" text="2021年" circle="true" :style="{position:'fixed', top:yearsposition[4]+'px',left:'280px'}" @click="gotoyear(yeartop[4])" customStyle="background-color: #f9f9f9; color:#000000; font-weight:bold; border: none;"></uni-tag>
		<uni-tag :class="[{nocheck:!slip}]" text="2022年" circle="true" :style="{position:'fixed', top:yearsposition[5]+'px',left:'280px'}" @click="gotoyear(yeartop[5])" customStyle="background-color: #f9f9f9; color:#000000; font-weight:bold; border: none;"></uni-tag>
		
		<uni-fab
		    :class="[{nocheck:!operation}]"
			:pattern="pattern"
			:content="content"
			horizontal="right"
			vertical="bottom"
			direction="horizontal"
			@trigger="trigger">
		</uni-fab>
				
		<view class="bottom"></view>
	
	</view>
</template>

<script>
    export default {
        data() {
            return {
				httpurl: getApp().globalData.weburl, //app.vue中定义的全局变量，运行前请前往修改
				allimglist:{},//后台返回的图片json
				operation: false ,//是否进入长按状态
				barHeight:20,//页面顶部高度
				allchecked:false,//是否全选
				slip:false,//屏幕是否滑动
				checkedimg:[] ,//选中的图片id  将它传给后台
				yeartop:[],
				yearsposition:[],
				bottomtop:0,
				showheight:0,
				windowWidth:0,
				windowHeight:0,
				pattern: {
					color: '#4b4b4b',
					backgroundColor: '#FFFFFF',
					selectedColor: '#FFFFFF',
					buttonColor:'#d3d3d3'
				},
				content: [
				{
					iconPath: '/static/icon/trash.png',
					//selectedIconPath: '/static/componentHL.png',
					text: '删除',
					active: false
				},
				{
					iconPath: '/static/icon/share.png',
					//selectedIconPath: '/static/apiHL.png',
					text: '分享',
					active: false
				},
				{
					iconPath: '/static/icon/upload.png',
					//selectedIconPath: '/static/templateHL.png',
					text: '上传',
					active: false
				}]
				
				
            }
			
        },
		computed:{
			gettime() {
			    return (time) => {
					var l=time.split('-');
					l[1]=l[1][0]=='0'? ' '+l[1][1]:l[1];
				    return l[0]+'年'+l[1]+'月'+l[2]+'日'
			    }
			},
			getyear() {
			    return (time) => {
					var l=time.split('-');
				    return 'year'+l[0]
			    }
			}
		},
        onLoad() {
		    this.getSystemStatusBarHeight();
			this.apiClick();
			var that=this;
			uni.getSystemInfo({
			    success: function (res) {
			        that.windowWidth=res.windowWidth;
			        that.windowHeight=res.windowHeight;
			    },
				fail:function(){
					console.log("屏幕宽高获取失败");
				}
			});
        },
		onReady() { //页面初次渲染完毕执行
		},
		mounted(){
			
			setTimeout(()=>{
			var view = uni.createSelectorQuery().select('.showview');
			view.boundingClientRect(data => {
			console.log(data);
			this.showheight=data.height;
			}).exec();
			},1000);
			
			//获取年份索引高度
			setTimeout(()=>{
			var yearlist=['2017','2018','2019','2020','2021','2022'];
			var yearstop=[];
			var position=[];
			for(let i=0;i<6;i++){ 
				var view = uni.createSelectorQuery().select('.year'+yearlist[i]);
				view.boundingClientRect(data => {
				console.log(data);
				yearstop.push(data.top);
				var p=data.top/this.showheight*this.windowHeight;
				if(p<250){
					p=p*4+110;
				}
				else if(p>700){
					p=p-30;
				}
				position.push(p);
				}).exec();
			}
			this.yeartop=yearstop;
			this.yearsposition=position;
			},1000);
			
		},
        methods:{
			 //获取系统状态栏高度
			getSystemStatusBarHeight:function(){
				// #ifdef APP-PLUS
				var height = plus.navigator.getStatusbarHeight();
				this.barHeight = height-5;
				// #endif
				// #ifdef H5
				this.barHeight = 0;
				// #endif
				console.log(this.barHeight);
			},
			imglongtap(){
				console.log(this.yeartop);
				console.log(this.yearsposition);
				console.log(this.windowHeight);
				this.operation=true;
			},
			closelongtap(){
				this.operation=false;
				this.allchecked=false;
				var items = this.allimglist;
				console.log(items);
				//结束多选模式，结束时将所有多选框清空
				for (var i = 0, lenI = items.length; i < lenI; ++i) {
					const item = items[i].imglist
					for(var j=0 , lenJ = item.length; j<lenJ; ++j){
						this.$set(item[j],'checked',false);
					}
				}
			},
			// 点击单个选择
		    checkboxChange: function (e) {
				var items = this.allimglist;
				var values = e.detail.value;//当前选择的图片id列表
				console.log('选中的图片id：'+values);
				for (var i = 0, lenI = items.length; i < lenI; ++i) {
					const item = items[i].imglist
					for(var j=0 , lenJ = item.length; j<lenJ; ++j){
						if(values.includes(item[j].id)){
							this.$set(item[j],'checked',true)
						}else{
							this.$set(item[j],'checked',false)
						}
					}
				}
				this.checkedimg=values;

			},
			showdetail(index){
				console.log('点击图片索引：'+index);//点击的图片链接
				//准备一个装图片路径的  数组imgs
				var imgs=[];
				var items = this.allimglist;
				for (var i = 0, lenI = items.length; i < lenI; ++i) {
					const item = items[i].imglist
					for(var j=0 , lenJ = item.length; j<lenJ; ++j){
						imgs.push(this.httpurl+item[j].img)
				
					}
				}
			    console.log(imgs);
				//调用预览图片的方法
				uni.previewImage({
					urls:imgs,
					current:index,
					indicator:'default',
					longPressActions: {
					    itemList: ['发送给朋友', '保存图片', '收藏','删除','更多信息'],
					    success: function(data) {
					        console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) + '张图片');
					    },
					    fail: function(err) {
					        console.log(err.errMsg);
					    }
					}
				});
			
			},
			apiClick(){
				console.log("发起请求");
				uni.request({
				    url: this.httpurl+'getAllimg/', //服务器地址+接口名
				    data: {
						text : "abv"
				    },
					method: 'GET',
				    header: {
						'X-Requested-With': 'XMLHttpRequest',
						"Accept": "application/json",
						"Content-Type": "application/json; charset=UTF-8"
					},
					dataType: 'json',
			
				    success: (res) => {
						console.log("request success");
				        //console.log(JSON.parse(JSON.stringify( res.data)));
				        //this.text = 'request success';
						this.allimglist = res.data["imgs"];
						
				    },
					fail: (res) => {
						console.log("fail");
				    },
				});
			},
            pictureClick () {
				console.log('点击选择图片')
                uni.chooseImage({
                    count: 10,
                    sizeType: ['original', 'compressed'],
                    sourceType: ['camera','album'],
                    success:(res)=> {
						//console.log(res.tempFilePaths);
                        // 预览图片
						//console.log(res.tempFiles[0]);
                        uni.previewImage({
                            urls: res.tempFilePaths,
                            longPressActions: {
                                itemList: ['发送给朋友', '保存图片', '收藏'],
                                success: function(data) {
                                    console.log('选中了第' + (data.tapIndex + 1) + '个按钮,第' + (data.index + 1) + '张图片');
                                },
                                fail: function(err) {
                                    console.log(err.errMsg);
                                }
                            }
                        });
					    //this.Imgs = res.tempFilePaths[0];
                    }
                    });	
			},
			searchClick(classname){
			    // const query = uni.createSelectorQuery().in(this);
			    // query.select('.x1').boundingClientRect(data => {
			    //    console.log("得到布局位置信息" + JSON.stringify(data));
			    //    //console.log("节点离页面顶部的距离为" + data.top);
			    // }).exec();
				setTimeout(()=>{
				var view = uni.createSelectorQuery().select('.'+classname);
				view.boundingClientRect(data => {
				console.log(data);
				//this.yeartop=data.top;
				return data.top;
				}).exec();
				},100);
			
			},
			//锚点跳转
			gotoyear(top){
				uni.pageScrollTo({
					  duration: 300,//过渡时间
					  scrollTop:top-108 ,//返回顶部的top值
				})
			},
			start(e) {
                //console.log("监测到屏幕滑动");
				this.slip=true;
			},
			end(e) {
				console.log("屏幕滑动停止");
				setTimeout(()=>{
				this.slip=false;
				},500);
			},
			trigger(e) {
				console.log(e);
				uni.showModal({
					title: '提示',
					content:`激活状态：${e.item.active};选中项：${e.item.text}`,
					success(res) {
						if (res.confirm) {
							console.log('用户点击确定');
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
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
	.rows{
		 display:block;
	}
	.colums{
		display: flex;
		flex-direction: column;
	}
	.nocheck{
		display: none;	
	}
	.check{
		font-size: 11px;
		line-height: 25px;
		position: absolute;
		right: 0;
		top: 0;
		z-Index: 2;
		padding: 0 2em;
		-webkit-transform-origin: left bottom;
		-moz-transform-origin: left bottom;
		transform-origin: left bottom;
		-webkit-transform: translate(29.29%, -100%);
		-moz-transform: translate(29.29%, -100%);
		transform: translate(43%, -13%);
		text-indent: 0;
		color: #ffffff;
	}
	.box-list {
	            position: relative;
	            float: left;
	            margin: 1px;
	            width: 127px;
	            height: 127px;
	            border-radius: 3px;
	            overflow: hidden;
				
	        }
	.topview{
		position:fixed; 
		top:0px;
	}
	.bottomview{
		position:fixed;
		bottom:0px;
	}
	.operation_btn{
		border: solid #007BFF 1px;
		border-radius: 5px;
        color: #007BFF;
	}
	.tag{
		background-color: #ffffff,
	}
</style>
