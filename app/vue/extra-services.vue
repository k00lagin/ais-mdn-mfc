<template>
	<at-table v-if="services" :columns="columns" :data="services.items"></at-table>
</template>

<script>
module.exports = {
		props: {
			mouType: String,
			mouId: String,
			cardData: Object,
		},
	data: function () {
		return {
			columns: [
				{
					title: 'Название',
					key: 'name'
				},
				{
					title: 'Да/Нет',
					key: 'is_provided'
				},
				{
					title: 'Комментарий',
					key: 'comment'
				},
				{
					title: 'Да/Нет',
					key: 'is_provided_mer'
				},
				{
					title: 'Комментарий куратора',
					key: 'comment_mer'
				},
			],
			services: undefined,
		}
	},
	methods: {
		getData: async function () {
			this.services = await app.asyncFetch({url:`dictionary_extra_services/list/${this.mouId}?_dc=1584815621511&page=1&start=0&limit=25`})
		}
	},
	mounted: function() {
		this.getData();
	}
}
</script>

<style>

</style>
