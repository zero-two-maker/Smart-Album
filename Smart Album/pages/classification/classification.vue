<template>
	<view>
		{{resInfo}}
		<image class="logo" :src="Imgs"></image>
		<button @click="getFile()">getSavedFileList</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				resInfo:"111",
				img:'file:///var/mobile/Containers/Data/Application/36FD19CE-DEED-4DFE-8B83-11A118B77AF0/Documents/Pandora/apps/HBuilder/doc/IMG_8137.JPG',
				Imgs: '/DCIM/107APPLE/IMG_7791.HEIC'
				}

		},
		methods: {
			getFile() {
				plus.gallery.pick
				const self = this;
				// 请求本地系统文件对象 plus.io.PRIVATE_WWW：应用运行资源目录常量
				plus.io.requestFileSystem(plus.io.PRIVATE_WWW, function(fobject){
					// fs.root是根目录操作对象DirectoryEntry
					fobject.root.getFile('IMG_8100.HEIC',{create:false}, function(fileEntry){
						fileEntry.file( function(file){
							var fileReader = new plus.io.FileReader();
							self.resInfo = JSON.stringify(file);
							self.Imgs="file://"+file["fullPath"];
							fileReader.readAsText(file, 'utf-8');
							fileReader.onloadend = function(evt) {
								self.resInfo = self.resInfo+'--'+JSON.stringify(evt);
								console.log(evt);
							}
							console.log(file.size+ '--' + file.name)
							self.resInfo = self.resInfo+'--'+file.size + '--' + file.name;
						});
					});
				});
		    }
	    }
    }
</script>

<style>

</style>
