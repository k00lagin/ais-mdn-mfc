<script>
module.exports = {
	props: {
		year: Number,
		month: Number,
		mouType: String,
		mouName: String,
		mouId: Number
	},
	data: function() {
		return {
			templateTypes: {
				mfc: ['federal','municipal','regional','otherServices','windowsAndEmployee'],
				urm: ['federal','otherServices','windowsAndEmployee']
			}
		}
	},
	components: {
		"download-template-button": httpVueLoader(
			"vue/download-template-button.vue"
		)
	}
};
</script>

<style>
	.template-bar {
		position: relative;
	}
	.update {
		display: none;
		position: absolute;
		left: 0;
		transform: translateX(-100%);
	}
	.template-bar:hover .update,
	.template-bar:focus-within .update {
		display: block;
	}
</style>

<template>
	<div class="template-bar" style="display:flex;">
		<button class="update" type="button" aria-label="Обновить данные о шаблонах">@</button>
		<download-template-button
			v-for="type in templateTypes[mouType]"
			v-bind:key="type"
			:year="Number(year)"
			:month="Number(month)"
			:mou-type="mouType"
			:mou-id="mouId"
			:mou-name="mouName"
			:template-type="type"
		></download-template-button>
	</div>
</template>
