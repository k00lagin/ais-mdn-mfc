<template>
	<div>
		<div id="toolPanel" style="padding:4px; display: flex; align-items: flex-end;">
			<div style="width: 100px; display: inline-block;">
				<at-select name="month"  v-model="month" size="small">
					<at-option :value="1">Январь</at-option>
					<at-option :value="2">Февраль</at-option>
					<at-option :value="3">Март</at-option>
					<at-option :value="4">Апрель</at-option>
					<at-option :value="5">Май</at-option>
					<at-option :value="6">Июнь</at-option>
					<at-option :value="7">Июль</at-option>
					<at-option :value="8">Август</at-option>
					<at-option :value="9">Сентябрь</at-option>
					<at-option :value="10">Октябрь</at-option>
					<at-option :value="11">Ноябрь</at-option>
					<at-option :value="12">Декабрь</at-option>
				</at-select>
			</div>
			<div style="width: 80px; display: inline-block;">
				<at-input v-model="year" size="small"></at-input>
			</div>
			<!--<div class="spacer"></div>
			<form @submit.prevent="uploadTemplate" id="uploadTemplate" method="post" action="https://xn--d1achjhdicc8bh4h.xn--p1ai/mfc/ws/quarterDataExcel/uploadData" enctype="multipart/form-data" target="_blank">
				<input id="uploadFilename" name="filename" type="hidden">
				<label class="at-btn at-btn--primary at-btn--small"><i class="icon icon-upload"></i>  Загрузить шаблон<input style="display:none" type="file" name="docFile" @change="handleFileSelect"></label>
			</form>-->
		</div>
		<table style="width: calc(100% - 24px); margin: 12px; box-sizing: border-box;">
			<template v-for="(upload, type) in uploads">
				<thead>
					<tr>
						<td colspan="5" style="font-weight: 700;">{{ templateTitles[type] }}</td>
					</tr>
					<tr style="font-weight: 500">
						<td>Дата выгрузки</td>
						<td>Выгрузил(а)</td>
						<td>Дата загрузки</td>
						<td>Загрузил(а)</td>
						<td>Файл</td>
					</tr>
				</thead>
				<tbody>
					<template v-if="upload && upload.items">
						<tr v-for="item in upload.items">
							<td>{{ item.template.createDate | datetime }}</td>
							<td>{{ item.template.userId }}</td>
							<td>{{ item.uploadFile.createDate | datetime }}</td>
							<td>{{ item.uploadFile.userId }}</td>
							<td>
								<a :href="'https://моидокументы.рф/mfc/ws/file/view/' + item.uploadFile.fileId">{{ item.uploadFile.fileName }}</a>
								<at-badge v-if="item.uploadFile.countErrors" :value="item.uploadFile.countErrors"></at-badge>
								<at-badge v-else status="success" value="✓"></at-badge>
							</td>
						</tr>
					</template>
				</tbody>
			</template>
		</table>
	</div>	
</template>
<script>
	module.exports = {
		watch: {
			'$route' (to, from) {
				this.mouId = to.params.mouId;
				this.mouType = to.params.mouType;
				this.getData();
			},
			month: function() {
				this.getData();
			},
			year: function() {
				this.getData();
			}
		},
		mounted: function() {
			this.getData();
		},
		props: {

		},
		data: function () {
			return {
				uploads: {
					federal: null,
					regional: null,
					municipal: null,
					otherServices: null,
					windowsAndEmployee: null
				},
				templateTitles: {
					federal: 'Федеральные услуги',
					regional: 'Региональные услуги',
					municipal: 'Муниципальные услуги',
					otherServices: 'Услуги иных организаций',
					windowsAndEmployee: 'Окна и сотрудники'
				},
				federalUploads: null,
				regionalUploads: null,
				municipalUploads: null,
				otherUploads: null,
				windowsUploads: null,
				mouId: null,
				mouType: null,
				month: null,
				year: null
			}
		},
		computed: {
		},
		methods: {
			getData() {
				if (!this.month || !this.year) {
					this.month = (new Date).getMonth() || 12;
					if (this.month == 12) {
						this.year = (new Date).getFullYear() - 1
					}
					else {
						this.year = (new Date).getFullYear();
					}						
				}
				this.mouId = this.$route.params.mouId;
				this.mouType = this.$route.params.mouType;
				app.fetchData('https://моидокументы.рф/mfc/ws/quarterDataExcel/getTemplatesList?rf_subject=80&year='+this.year+'&month='+this.month+'&formType=federal&importType=mou&mouType=' + this.mouType + '&mouId=' + this.mouId, this.$data.uploads, 'federal');
				app.fetchData('https://моидокументы.рф/mfc/ws/quarterDataExcel/getTemplatesList?rf_subject=80&year='+this.year+'&month='+this.month+'&formType=regional&importType=mou&mouType=' + this.mouType + '&mouId=' + this.mouId, this.$data.uploads, 'regional');
				app.fetchData('https://моидокументы.рф/mfc/ws/quarterDataExcel/getTemplatesList?rf_subject=80&year='+this.year+'&month='+this.month+'&formType=municipal&importType=mou&mouType=' + this.mouType + '&mouId=' + this.mouId, this.$data.uploads, 'municipal');
				app.fetchData('https://моидокументы.рф/mfc/ws/quarterDataExcel/getTemplatesList?rf_subject=80&year='+this.year+'&month='+this.month+'&formType=otherServices&importType=mou&mouType=' + this.mouType + '&mouId=' + this.mouId, this.$data.uploads, 'otherServices');
				app.fetchData('https://моидокументы.рф/mfc/ws/quarterDataExcel/getTemplatesList?rf_subject=80&year='+this.year+'&month='+this.month+'&formType=windowsAndEmployee&importType=mou&mouType=' + this.mouType + '&mouId=' + this.mouId, this.$data.uploads, 'windowsAndEmployee');
			}
		}
	};	
</script>
