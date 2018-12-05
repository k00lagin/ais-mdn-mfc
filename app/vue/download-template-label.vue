<template v-if="uploads">
		<div class="success" v-if="errors == 0"></div>
		<div  class="error" v-else-if="errors > 0"></div>
		<div v-else></div>
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
				errors: null
			}
		},
		methods: {
			getData: function () {
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
	.success {
		width:100%;
		height:2px;
		background:green;
	}
	.error {
		width:100%;
		height:2px;
		background:red;
	}
</style>