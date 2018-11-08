<template>
	<div>
		<table>
			<thead>
				<tr>
					<th></th>
					<th>Выходной</th>
					<th></th>
					<th>Без перерыва</th>
					<th></th>
				</tr>
			</thead>
			<tr v-for="day in week">
				<td>{{ day.label }}</td>
				<td><input type="checkbox" :checked="schedule[day.shortName + '_weekend']"></td>
				<template v-if="!schedule[day.shortName + '_weekend']">
					<td><label>с</label><input type="time" :value="schedule[day.shortName + '_from']"><label for="">до</label><input type="time" :value="schedule[day.shortName + '_to']"></td>
					<td><input type="checkbox" :checked="schedule[day.shortName + '_no_breakfast']"></td>
					<td v-if="!schedule[day.shortName + '_no_breakfast']">
						<label>с</label><input type="time" :value="schedule[day.shortName + '_breakfast_from']"><label for="">до</label><input type="time" :value="schedule[day.shortName + '_breakfast_to']">				
					</td>
				</template>
				<br>
			</tr>			
		</table>
	</div>
</template>

<script>
	module.exports = {
        props: {
            schedule: Object
        },
        data: function () {
            return {
                week: [
                    {
                        label: 'Понедельник:',
                        shortName: 'mo'
                    },
                    {
                        label: 'Вторник:',
                        shortName: 'tu'
                    },
                    {
                        label: 'Среда:',
                        shortName: 'we'
                    },
                    {
                        label: 'Четверг:',
                        shortName: 'th'
                    },
                    {
                        label: 'Пятница:',
                        shortName: 'fr'
                    },
                    {
                        label: 'Суббота:',
                        shortName: 'sa'
                    },
                    {
                        label: 'Воскресенье:',
                        shortName: 'su'
                    }
                ]
            }
        }
	};
</script>

<style>
	
</style>