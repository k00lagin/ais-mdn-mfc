<script>
module.exports = {
	template: "#template-grid-template",
	mixins: [commonMethods],
	props: {
		data: Array,
		columns: Array,
		favoriteList: Object,
		filterKey: String,
		year: Number,
		month: Number,
		showFavoritesOnly: Boolean
	},
	data: function() {
		var sortOrders = {};
		this.columns.forEach(function(key) {
			sortOrders[key.field] = 1;
		});
		return {
			sortKey: "",
			sortOrders: sortOrders
		};
	},
	computed: {
		dictionarySubjectName: function() {
			return app.dictionarySubjectName;
		},
		// showFavoritesOnly: function() {
		// 	return this.$store.state.showFavoritesOnly;
		// },
		filteredData: function() {
			// let showFavoritesOnly = this.$store.state.showFavoritesOnly;
			let showFavoritesOnly = this.showFavoritesOnly;
			var sortKey = this.sortKey;
			var filterKey = this.filterKey && this.filterKey.toLowerCase();
			var favoriteList = this.favoriteList;
			var order = this.sortOrders[sortKey] || 1;
			var data = this.data;
			if (data) {
				if (showFavoritesOnly) {
					data = data.filter(function(item) {
						return favoriteList && favoriteList[item.id] == true;
					});
				}
				if (filterKey) {
					data = data.filter(function(row) {
						return Object.keys(row).some(function(key) {
							return (
								String(row[key])
									.toLowerCase()
									.indexOf(filterKey) > -1
							);
						});
					});
				}
				if (!showFavoritesOnly) {
					var favoriteData = data.filter(function(item) {
						return favoriteList && favoriteList[item.id] == true;
					});
					var nonFavoriteData = data.filter(function(item) {
						return favoriteList && favoriteList[item.id] != true;
					});
					if (sortKey) {
						var favoriteData = favoriteData.sort(function(a, b) {
							a = a[sortKey];
							b = b[sortKey];
							return (a === b ? 0 : a > b ? 1 : -1) * order;
						});
						nonFavoriteData = nonFavoriteData.sort(function(a, b) {
							a = a[sortKey];
							b = b[sortKey];
							return (a === b ? 0 : a > b ? 1 : -1) * order;
						});
					}
					data = favoriteData.concat(nonFavoriteData);
				} else {
					if (sortKey) {
						data = data.sort(function(a, b) {
							a = a[sortKey];
							b = b[sortKey];
							return (a === b ? 0 : a > b ? 1 : -1) * order;
						});
					}
				}
			}
			return data;
		}
	},
	filters: {
		capitalize: function(str) {
			return str.charAt(0).toUpperCase() + str.slice(1);
		}
	},
	methods: {
		sortBy: function(key) {
			this.sortKey = key;
			this.sortOrders[key] = this.sortOrders[key] * -1;
		},
		handleFavoriteClick: function(e) {
			var id = e.target.parentNode.parentNode.parentNode.id;
			var newValue = app.favoriteList;
			if (e.target.checked) {
				newValue[id] = e.target.checked;
			} else {
				delete newValue[id];
			}
			app.favoriteList = newValue;
		}
	},
	created: function() {
		//console.log('COLUMNS IN GRID - ' + this.columns);
	},
	components: {
		"download-template-button": httpVueLoader(
			"vue/download-template-button.vue"
		),
		"fill-templates": httpVueLoader("vue/fill-templates.vue")
	}
};
</script>
<template type="text/x-template" id="template-grid-template">
	<table :class="{ 'favorite-only' : showFavoritesOnly }">
		<thead>
			<tr>
				<th
					v-for="(key, index) in columns"
					@click="!key.noSort ? sortBy(key.field) : ''"
					:class="'column__' + key.field + ' ' + { active: sortKey == key.field }"
				>
					{{ key.label }}
					<span
						v-if="!key.noSort && sortKey == key.field"
						class="arrow"
						:class="sortOrders[key.field] > 0 ? 'asc' : 'dsc'"
					></span>
				</th>
			</tr>
		</thead>
		<tbody>
			<tr v-for="entry in filteredData" :id="entry.id" v-bind:key="entry.id">
				<td v-for="key in columns" :class="'column__' + key.field" v-bind:key="key.field">
					<label v-if="key.label == ' '" class="is_favorite" :title="favoriteList[entry.id] == true?'Удалить из избранного':'Добавить в избранное'">
						<input
							type="checkbox"
							@change="handleFavoriteClick"
							:checked="favoriteList[entry.id] == true"
						/>
						<div></div>
					</label>
					<div v-else-if="key.label == 'Шаблоны'" style="display:flex;">
						<download-template-button
							v-for="type in entry.mouType == 'mfc' ? ['federal','municipal','regional','otherServices','windowsAndEmployee'] : ['federal','otherServices','windowsAndEmployee']"
							v-bind:key="type"
							:year="Number(year)"
							:month="Number(month)"
							:mou-type="entry.mouType"
							:mou-id="entry.id"
							:mou-name="entry.shortName || entry.full_name"
							:template-type="type"></download-template-button>
					</div>
					<template
						v-else-if="key.field == 'urban_area' || key.field == 'urban_district'"
					>{{ dictionarySubjectName[entry[key.field]] }}</template>
					<template v-else-if="key.field == 'address'">{{ entry['shortAddress'] || entry['address'] }}</template>
					<template v-else-if="key.field == 'full_name'">
						<router-link
							v-if="$route.path == '/mou_template_list'"
							:to="'/quarter_data_import/' + entry.mouType + '/' + entry.id + '/' + year + '/' + month"
							class="textBtn"
						>{{ (showFavoritesOnly && entry.mouType ==='urm' && entry['shortName']) ? entry['shortName'].replace(entry['shortName'].match(/[А-ЯЁ][А-ЯЁа-яё-]+ района/), '') : entry['shortName'] || entry[key.field] }}</router-link>
						<router-link
							v-if="$route.path == '/mou_common_data'"
							:to="'/common_data/' + entry.mouType + '/' + entry.id"
							class="textBtn"
						>{{ (showFavoritesOnly && entry.mouType ==='urm' &&  entry['shortName']) ? entry['shortName'].replace(entry['shortName'].match(/[А-ЯЁ][А-ЯЁа-яё-]+ района/), '') : entry['shortName'] || entry[key.field] }}</router-link>
					</template>
					<template v-else-if="key.field == 'start_date'">{{ entry[key.field] | date }}</template>
					<span v-else>{{ entry[key.field] }}</span>
				</td>
			</tr>
		</tbody>
	</table>
</template>
