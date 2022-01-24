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
				resInfo:" ",
				Imgs: '/static/image0.png'
			}
		},
		methods: {
			getFile() {
				const self = this;
				// 请求本地系统文件对象 plus.io.PRIVATE_WWW：应用运行资源目录常量
				plus.io.requestFileSystem(plus.io.PUBLIC_DOCUMENTS, function(fobject){
					// fs.root是根目录操作对象DirectoryEntry
					fobject.root.getFile('IMG_8100',{create:false}, function(fileEntry){
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
