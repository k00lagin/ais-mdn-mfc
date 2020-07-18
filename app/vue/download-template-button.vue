<template>
	<at-popover trigger="hover" placement="bottom" :title="countErrors != null ? templateTypeLocale[templateType]: undefined" :content="countErrors == null ? templateTypeLocale[templateType]: undefined">
		<button type="button" class="at-tag__text downloadTemplateLabel" @click="handleDownloadClick"
			v-bind:class="{ hasData: countErrors != null, 'at-tag--success': countErrors == 0, 'at-tag--error': countErrors > 0 }" >
			{{ templateTypeLocale[templateType].substr(0, 1) }}
		</button>
		<template slot="content" v-if="countErrors != null">
			<p>Дата загрузки: {{ this.uploads.items[0].template.createDate | datetime }}</p>
			<div v-if="countErrors > 0">
				<div v-for="(error, key) in this.uploads.items[0].uploadFile.errors" v-bind:key="key">{{ error }}</div>
			</div>
		</template>
	</at-popover>
</template>

<script>
	module.exports = {
		props: {
			year: Number,
			month: Number,
			templateType: String,
			mouType: String,
			mouName: String,
			mouId: Number,
		},
		data: function() {
			return {
				uploads: null,
				countErrors: null,
				templateTypeLocale: {
					"federal": "Федеральные услуги",
					"municipal": "Муниципальные услуги",
					"regional": "Региональные услуги",
					"otherServices": "Иные услуги",
					"windowsAndEmployee": "Окна и сотрудники"
				}
			}
		},
		methods: {
			handleDownloadClick: function() {
				dialog.showSaveDialog(null, {
					title: 'Выберите место сохранения шаблона',
					defaultPath: `${this.templateTypeLocale[this.templateType]} ${this.mouName}.xls`
				}).then(response => {
					if (!response.canceled) {
						app.fetchAndSave('https://xn--d1achjhdicc8bh4h.xn--p1ai/mfc/ws/quarterDataExcel/templateMou/' + this.mouId + '?year=' + this.year + '&month=' + this.month + '&serviceType=federal&mouType=' + this.mouType + '&subjectId=80',
							response.filePath);
					}
				})
			},
			getData: function () {
				this.countErrors = null;
				this.uploads = null;
				if (typeof(this.year) == "number" && typeof(this.month) == "number") {
					app.fetchData('https://моидокументы.рф/mfc/ws/quarterDataExcel/getTemplatesList?rf_subject=80&year='+this.year+'&month='+this.month+'&formType='+this.templateType+'&importType=mou&mouType=' + this.mouType + '&mouId=' + this.mouId, this.$data, "uploads");
				}
			}
		},
		mounted: function() {
			if (app.favoriteList[this.mouId] == true) {
				this.getData();
			}
		},
		watch: {
			'$route' (to, from) {
				if (app.favoriteList[this.mouId] == true) {
					this.getData();
				}
			},
			month: function() {
				if (app.favoriteList[this.mouId] == true) {
					this.getData();
				}
			},
			year: function() {
				if (app.favoriteList[this.mouId] == true) {
					this.getData();
				}
			},
			uploads: function () {
				if (this.uploads && this.uploads.items && this.uploads.items[0] && this.uploads.items[0].uploadFile) {
					this.countErrors = this.uploads.items[0].uploadFile.countErrors;
				}
				else {
					this.countErrors = null;
				}
			}
		},
	};
</script>
<style>
	.downloadTemplateLabel {
		cursor: pointer;
		border-style: solid;
		border-width: 1px;
		padding: 2px 4px;
	}
	.downloadTemplateLabel:first-child {
		border-top-left-radius: 2px;
		border-bottom-left-radius: 2px;
	}
	.downloadTemplateLabel:last-child {
		border-top-right-radius: 2px;
		border-bottom-right-radius: 2px;
	}
	.downloadTemplateLabel:hover {
		background-image: linear-gradient(rgba(0, 0, 0, .1),rgba(0, 0, 0, .1));
	}
	.downloadTemplateLabel.hasData:hover {
		background-image: linear-gradient(rgba(255, 255, 255, .1),rgba(255, 255, 255, .2));
	}
</style>
