<template>
	<at-tag class="downloadTemplateLabel" v-if="errors == null">{{ templateTypeLocale[templateType] }}</at-tag>
	<at-tag class="downloadTemplateLabel hasData" v-else :color="errors > 0?'error':'success'">{{ templateTypeLocale[templateType] }}</at-tag>	
</template>

<script>
	module.exports = {
		props: {
			year: Number,
			month: Number,
			templateType: String,
			mouType: String,
			mouId: Number,
			templateType: String
		},
		data: function() {
			return {
				uploads: null,
				errors: null,
				templateTypeLocale: {
					"federal": "Ф",
					"municipal": "М",
					"regional": "Р",
					"otherServices": "И",
					"windowsAndEmployee": "О"
				}
			}
		},
		methods: {
			getData: function () {
				this.errors = null;
				this.uploads = null;
				if (typeof(this.year) == "number" && typeof(this.month) == "number") {
					app.fetchData('https://моидокументы.рф/mfc/ws/quarterDataExcel/getTemplatesList?rf_subject=80&year='+this.year+'&month='+this.month+'&formType='+this.templateType+'&importType=mou&mouType=' + this.mouType + '&mouId=' + this.mouId, this.$data, "uploads");
				}				
			}
		},
		mounted: function() {
			this.getData();
		},
		watch: {
			'$route' (to, from) {
				this.getData();
			},
			month: function() {
				this.getData();
			},
			year: function() {
				this.getData();
			},
			uploads: function () {
				if (this.uploads && this.uploads.items && this.uploads.items[0] && this.uploads.items[0].uploadFile) {
					this.errors = this.uploads.items[0].uploadFile.countErrors;
				}
				else {
					this.errors = null;
				}
			}
		},
	};
</script>
<style>
	.downloadTemplateLabel {
		cursor: pointer;
	}
	.downloadTemplateLabel:hover {
		background-image: linear-gradient(rgba(0, 0, 0, .1),rgba(0, 0, 0, .1));
	}
	.downloadTemplateLabel.hasData:hover {
		background-image: linear-gradient(rgba(255, 255, 255, .1),rgba(255, 255, 255, .2));
	}
</style>