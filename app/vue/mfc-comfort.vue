<template>
	<div>
		<div class="cardElement" v-for="field in fields" style="font-size:11px;">
			<h2 v-if="field.type == 'subheading'">{{ field.label }}</h2>
			<template v-else-if="field.type == 'checkbox'">
				<label :for="field.field">{{ field.label }}</label><input :type="field.type || 'text'" :checked="cardData.mfc_comfort[field.field]" v-on:change="cardData.mfc_comfort[field.field] = $event.target.value">
			</template>
			<template v-else>
				<label :for="field.field">{{ field.label }}</label><input :type="field.type || 'text'" :value="cardData.mfc_comfort[field.field]" v-on:input="cardData.mfc_comfort[field.field] = $event.target.value">
			</template>
		</div>
	</div>
		<!--<template v-else-if="field.type == 'select'">
			<label :for="field.field">{{ field.label }}</label>
			<select :name="field.field" :id="field.field">
				<option v-for="option in field.options" :value="option.value" :selected="option.value == cardData.ais[field.field]">{{ option.label || option.value }}</option>
			</select>
		</template>
		<template v-else-if="field.type == 'schedule'">
			<schedule :schedule="cardData.ais[field.field]"></schedule>	
		</template>

	</div>-->
</template>

<script>
	module.exports = {
		data: function() {
			return({
				fields: [
					{
						type: 'subheading',
						label: 'Сведения о комфортности и доступности в МФЦ'
					},
					{
						label: 'Обеспечивается возможность приема от заявителей денежных средств в счет уплаты государственной пошлины или иной платы за предоставление государственных и муниципальных услуг, взимаемых в соответствии с законодательством Российской Федерации',
						field: 'pay_in_mfc',
						type: 'checkbox'
					},
					{
						label: 'Перечень государственных и муниципальных услуг, предоставление которых организовано в МФЦ',
						field: 'stand_service_list',
						type: 'checkbox'
					},
					{
						label: 'Сроки предоставления государственных и муниципальных услуг',
						field: 'stand_service_dates',
						type: 'checkbox'
					},
					{
						label: 'Размеры государственной пошлины и иных платежей, уплачиваемых заявителем при получении государственных и муниципальных услуг, порядок их уплаты',
						field: 'stand_fee_size',
						type: 'checkbox'
					},
					{
						label: 'Информацию о дополнительных (сопутствующих) услугах, а также об услугах, необходимых и обязательных для предоставления государственных и муниципальных услуг, размерах и порядке их оплаты',
						field: 'stand_extra_services',
						type: 'checkbox'
					},
					{
						label: 'Порядок обжалования действий (бездействия), а также решений органов, предоставляющих государственные услуги, и органов, предоставляющих муниципальные услуги, государственных и муниципальных служащих, МФЦ, работников МФЦ',
						field: 'stand_appealing_procedure',
						type: 'checkbox'
					},
					{
						label: 'Информацию о предусмотренной законодательством РФ ответственности должностных лиц органов, предоставляющих государственные услуги, должностных лиц органов, предоставляющих муниципальные услуги, работников МФЦ, работников привлекаемых организаций за нарушение порядка предоставления государственных и муниципальных услуг',
						field: 'stand_responsibility_info',
						type: 'checkbox'
					},
				],
			});
		},
		props: {
			mouType: String,
			mouId: String,
			cardData: Object,
		},
		created: function() {
		},
	};
</script>

<style>
	
</style>
