<template>
	<div>
		<div class="cardElement" v-for="field in fields" style="font-size:11px;">
			<h2 v-if="field.type == 'subheading'">{{ field.label }}</h2>
			<template v-else>
				<label :for="field.field">{{ field.label }}</label><input :type="field.type || 'text'" :value="cardData.ais[field.field]" v-on:input="cardData.ais[field.field] = $event.target.value">
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
						label: 'Сведения об АИС поддержки деятельности МФЦ'
					},
					{
						label: 'Наименование АИС МФЦ',
						field: 'ais_name'
					},
					{
						label: 'Организация-разработчик',
						field: 'developer'
					},
					{
						label: 'Наименование АИС, обеспечивающей подключение к СМЭВ',
						field: 'connectNameSMEV'
					},
					{
						label: 'Мнемоника АИС МФЦ в СМЭВ',
						field: 'mnemonicSMEV'
					},
					{
						label: 'Правообладатель',
						field: 'rightholder'
					},
					{
						label: 'Версия',
						field: 'version'
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
