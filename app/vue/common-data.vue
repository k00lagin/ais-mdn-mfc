<template>
	<div>
		<at-tabs type="card" v-model="activeTab" size="small">
			<at-tab-pane label="Общие данные" name="name1">
				<template>
					<div class="cardElement" v-for="field in cardFields" style="font-size:11px;">
						<h2 v-if="field.type == 'subheading'">{{ field.label }}</h2>
						<template v-else-if="field.type == 'select'">
							<label :for="field.field">{{ field.label }}</label>
							<select :name="field.field" :id="field.field">
								<option v-for="option in field.options" :value="option.value" :selected="option.value == cardData.common_data[field.field]">{{ option.label || option.value }}</option>
							</select>
						</template>
						<template v-else-if="field.type == 'schedule'">
							<schedule :schedule="cardData.common_data[field.field]"></schedule>	
						</template>
						<template v-else>
							<label :for="field.field">{{ field.label }}</label><input :type="field.type || 'text'" :value="cardData.common_data[field.field]" v-on:input="cardData.common_data[field.field] = $event.target.value">
						</template>
					</div>
				</template>
			</at-tab-pane>
			<at-tab-pane label="АИС МФЦ" name="name2">
				<ais-mfc :mou-type="mouType" :mou-id="mouId" :card-data="cardData"></ais-mfc>
			</at-tab-pane>
			<at-tab-pane label="Комфортность и доступность" name="name3">
				<mfc-comfort :mou-type="mouType" :mou-id="mouId" :card-data="cardData"></mfc-comfort>
			</at-tab-pane>
			<at-tab-pane label="IT-инфраструктура МФЦ" name="name4">
				<p>Тут будет про IT-инфраструктуру</p>
			</at-tab-pane>
			<at-tab-pane label="Бренд МФЦ" name="name5">
				<p>Тут будет про Бренд</p>
			</at-tab-pane>
		</at-tabs>
	</div>	
</template>
<script>
	module.exports = {
		components: {
			'schedule': httpVueLoader('vue/schedule.vue'),
			'ais-mfc': httpVueLoader('vue/ais-mfc.vue'),
			'mfc-comfort': httpVueLoader('vue/mfc-comfort.vue'),
		},
		data: function() {
			return({
				mouType: null,
				mouId: null,
				cardData: null,
				activeTab: null,
				cardFields: [
					{
						label: 'Полное наименование',
						field: 'full_name'
					},
					{
						label: 'Организационно-правовая форма',
						field: 'business_form',
						type: 'select',
						options: [
							{
								value: 'Государственное бюджетное учреждение'
							},
							{
								value: 'Государственное казенное учреждение'
							},
							{
								value: 'Государственное автономное учреждение'
							},
							{
								value: 'Муниципальное бюджетное учреждение'
							},
							{
								value: 'Муниципальное казенное учреждение'
							},
							{
								value: 'Муниципальное автономное учреждение'
							}
						]
					},
					{
						label: 'ИНН',
						field: 'inn'
					},
					{
						label: 'ОГРН',
						field: 'mfc_ogrn'
					},
					{
						label: 'Статус',
						field: 'status',
						type: 'select',
						options: [
							{
								value: 'действующий'
							},
							{
								value: 'ликвидированный'
							}
						]
					},
					{
						label: 'Дата начала предоставления услуг',
						type: 'date',
						field: 'start_date'
					},
					{
						type: 'number',
						label: 'Количество окон',
						field: 'wnd_count'
					},
					{
						type: 'number',
						label: 'Количество окон, в которых работают универсальные специалисты',
						field: 'universal_wnd_count'
					},
					{
						label: 'Тип создания',
						field: 'create_type',
						type: 'select',
						options: [
							{
								value: 'безвозмездное пользование'
							},
							{
								value: 'строительство'
							},
							{
								value: 'ремонт'
							},
							{
								value: 'аренда'
							},
							{
								value: 'приобретение помещения'
							}
						]
					},
					{
						label: 'Правовой статус',
						field: 'legal_status'
					},
					{
						label: 'Учредитель',
						field: 'mfc_founder'
					},
					{
						type: 'subheading',
						label: 'Руководитель'
					},
					{
						label: 'Ф.И.О.',
						field: 'head_full_name'
					},
					{
						label: 'Должность',
						field: 'head_position'
					},
					{
						label: 'Телефон',
						field: 'head_phone'
					},
					{
						label: 'Адрес электронной почты',
						field: 'head_email'
					},
					{
						type: 'subheading',
						label: 'Адрес'
					},
					{
						label: 'Полный адрес',
						field: 'address'
					},
					{
						label: 'Субъект РФ',
						field: 'rf_subject'
					},
					{
						label: 'Широта',
						field: 'coordinates.latitude'
					},
					{
						label: 'Долгота',
						field: 'coordinates.longitude'
					},
					{
						type: 'subheading',
						label: 'Режим работы'
					},
					{
						type: 'schedule',
						field: 'schedule'
					},
					{
						type: 'subheading',
						label: 'Контакты'
					},
					{
						label: 'Интернет-сайт',
						field: 'site'
					},
					{
						label: 'Телефон',
						field: 'phone'
					},
					{
						label: 'Адрес электронной почты',
						field: 'email'
					},
					{
						type: 'subheading',
						label: 'Контактное лицо'
					},
					{
						label: 'Ф.И.О. контактного лица',
						field: 'contact_person_full_name'
					},
					{
						label: 'Должность',
						field: 'contact_person_position'
					},
					{
						label: 'Телефон',
						field: 'contact_person_phone'
					},
					{
						label: 'Адрес электронной почты',
						field: 'contact_person_email'
					}
				],
			});
		},
		created: function() {
			this.mouId = this.$route.params.mouId;
			this.mouType = this.$route.params.mouType;
			this.getCardData(this.mouType, this.mouId)
		},
		methods: {
			getCardData: function(mouType, mouId) {
				let self = this;
				self.cardData = null;
				fetch('https://моидокументы.рф/mfc/ws/cards/' + mouType + '/getSingle/' + mouId, {
					credentials: "same-origin"
				})
				.then((response) => {
					if(!response.ok) {
						throw new Error('Network response was not ok');
					}
					response.json().then(function(data) {
						console.log(data);
						self.cardData = data;
					});
				})
				.catch((error) => {
					throw new Error(error);
				});
			},
		},
	};	
</script>
