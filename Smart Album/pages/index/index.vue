<template>
    <view class="content">
        <image class="logo" :src="Imgs"></image>
        <view class="text-area">
            <button class="title" @click="pictureClick()">{{title}}</button>
			<button  @click="allpictureClick()">打开文件</button>
        </view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                title: '调用相机',
                Imgs: '/static/stars.png'
            }
        },
        onLoad() {
		
        },
        methods:{
            // 调用手机相机
			allpictureClick(){
				console.log("111");
				plus.io.getFileInfo({filePath:"DCIM/108APPLE/",digestAlgorithm: "md5",success: (res) => {
					console.log(res)
					
				}});
				plus.io.resolveLocalFileSystemURL("DCIM/108APPLE/", function( entry ) {
						// 可通过entry对象操作test.html文件 
						entry.file( function(file){
							var fileReader = new plus.io.FileReader();
							alert("getFile:" + JSON.stringify(file));
							fileReader.readAsText(file, 'utf-8');
							fileReader.onloadend = function(evt) {
								alert("11" + evt);
								alert("evt.target" + evt.target);
								alert(evt.target.result);
							}
							alert(file.size + '--' + file.name);
						} );
					}, function ( e ) {
						alert( "Resolve file URL failed: " + e.message );
					} );
			},
            pictureClick () {
                uni.chooseImage({
                    count: 10,
                    sizeType: ['original', 'compressed'],
                    sourceType: ['camera','album'],
                    success:(res)=> {
                        // 预览图片
						console.log(res.tempFiles[0]);
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
					    this.Imgs = res.tempFilePaths[0];
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
</style>
