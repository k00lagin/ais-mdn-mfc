<template>
	<form @submit.prevent="uploadTemplate" id="uploadTemplate" method="post" action="https://xn--d1achjhdicc8bh4h.xn--p1ai/mfc/ws/quarterDataExcel/uploadData" enctype="multipart/form-data" target="_blank">
		<input id="uploadFilename" name="filename" type="hidden">
		<label class="at-btn at-btn--primary at-btn--small"><i class="icon icon-upload"></i>  Загрузить шаблон<input style="display:none" type="file" name="docFile" @change="handleFileSelect"></label>
	</form>
</template>
<script>
	module.exports = {
		methods: {
			handleFileSelect: function(e) {
				document.getElementById('uploadFilename').value=e.target.files[0].name;
				this.uploadTemplate();
			},
			uploadTemplate: function() {
				let form = document.getElementById('uploadTemplate');
				app.sendMultiformData('https://xn--d1achjhdicc8bh4h.xn--p1ai/mfc/ws/quarterDataExcel/uploadData', form);
			}
		}
	};
</script>