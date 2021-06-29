<script>
module.exports = {
	template: "#template-view",
	mixins: [commonMethods],
	components: {
		"upload-template": httpVueLoader("./upload-template.vue"),
		"template-grid": httpVueLoader("./template-grid.vue")
	},
	props: {
		list: String,
		columns: String
	},
	data: function() {
		return {
			searchQuery: "",
			month: null,
			year: null
		};
	},
	methods: {
		toggleFavorites: function(e) {
			this.$store.commit('toggleFavorites');
		},
		handleFileSelect: function(e) {
			document.getElementById("uploadFilename").value = e.target.files[0].name;
			this.uploadTemplate();
		},
		uploadTemplate: function() {
			let form = document.getElementById("uploadTemplate");
			this.sendMultiformData(
				"https://xn--d1achjhdicc8bh4h.xn--p1ai/mfc/ws/quarterDataExcel/uploadData",
				form
			);
		}
	},
	computed: {
		gridColumns: function() {
			return app[this.columns];
		},
		gridData: function() {
			return app[this.list];
		},
		favoriteList: function() {
			return app.favoriteList;
		},
		dictionarySubjectName: function() {
			return app.dictionarySubjectName;
		},
		showFavoritesOnly: function() {
			return this.$store.state.showFavoritesOnly;
		}
	},
	mounted: function() {
		this.month = new Date().getMonth() || 12;
		if (this.month == 12) {
			this.year = new Date().getFullYear() - 1;
		} else {
			this.year = new Date().getFullYear();
		}
	},
	created: function() {
		if (!app.mfcList) {
			app.getMfcList();
		}
		if (!app.urmList) {
			app.getUrmList();
		}
	}
};
</script>
<template>
	<div>
		<div v-if="this.columns === 'templateListColumns'">
			<div id="toolPanel" style="padding:4px; display: flex; align-items: flex-end;">
				<label class="at-checkbox__label" style="padding-right: 8px;padding-left: 8px;align-self: center;">
					<span class="at-checkbox__input"></span>
					<input type="checkbox" v-bind:checked="showFavoritesOnly"	@change="toggleFavorites">
					Только избранное
				</label>
				<div style="width: 200px; display: inline-block;">
					<at-input v-model="searchQuery" size="small">
						<template slot="prepend">
							<i class="icon icon-filter"></i>
						</template>
					</at-input>
				</div>
				<div style="width: 100px; display: inline-block;">
					<at-select name="month" v-model="month" size="small">
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
				<div class="spacer"></div>
				<upload-template></upload-template>
			</div>

			<template-grid
				:data="gridData"
				:columns="gridColumns"
				:filter-key="searchQuery"
				:favorite-list="favoriteList"
				:show-favorites-only="showFavoritesOnly"
				:year="year"
				:month="month"
			></template-grid>
		</div>
		<div v-if="this.columns === 'commonDataColumns'">
			<div id="toolPanel" style="padding:4px; display: flex; align-items: flex-end;">
				<label class="at-checkbox__label" style="padding-right: 8px;padding-left: 8px;align-self: center;">
					<span class="at-checkbox__input"></span>
					<input type="checkbox" v-bind:checked="showFavoritesOnly"	@change="toggleFavorites">
					Только избранное
				</label>
				<div style="width: 200px; display: inline-block;">
					<at-input v-model="searchQuery" size="small">
						<template slot="prepend">
							<i class="icon icon-filter"></i>
						</template>
					</at-input>
				</div>
			</div>
			<template-grid
				:data="gridData"
				:show-favorites-only="showFavoritesOnly"
				:columns="gridColumns"
				:filter-key="searchQuery"
				:favorite-list="favoriteList"
			></template-grid>
		</div>
	</div>
</template>
