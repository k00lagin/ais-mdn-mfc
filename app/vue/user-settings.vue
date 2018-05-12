<template>
	<div>
		<div v-for="field in userSettings">
			<template :for="field.field">
				<label v-if="field.field == 'fio'">
					{{ field.label }}
					<input v-if="isEditFio" v-model.trim="fio" type="text" style="width: 40ex;">
					<span v-else>{{ ' ' + currentUser[field.field] }}</span>
					<at-button v-if="!isEditFio" @click="isEditFio=true" icon="icon-edit" circle size="smaller"></at-button>
					<at-button v-if="isEditFio" @click="saveNewFio" icon="icon-save" circle size="smaller"></at-button>
					<at-button v-if="isEditFio" @click="cancelEditFio" icon="icon-x" circle size="smaller"></at-button>
				</label>
				<label v-else>
					{{ field.label + ' ' + currentUser[field.field] }}
				</label>
			</template>
		</div>
		<div v-if="isEditPassword">
			<label style="position: relative; display: inline-block;">
				<input id="userPassword" v-model="password" :type="isPasswordMasked?'password':'text'">
				<i @mousedown.left="unmaskPassword" @mouseup="maskPassword" @mouseout="maskPassword" :class="isPasswordMasked?'icon icon-eye-off':'icon icon-eye'" style="transform: translateY(-50%); position: absolute; right: 6px; top: 50%;"></i>
			</label>
			<at-button @click="saveNewPassword" icon="icon-save" circle size="small"></at-button>
			<at-button @click="cancelEditPassword" icon="icon-x" circle size="small"></at-button>
		</div>
		<at-button v-if="!isEditPassword" size="small"  @click="isEditPassword=true">Сменить пароль</at-button>
	</div>
</template>

<script>
	module.exports = {
		props: {

		},
		computed: {
			userSettings: function() {
				return(app.userSettings);
			},
			currentUser: function() {
				return(app.currentUser);
			},
		},
		data: function () {
	    return {
			isEditFio: false,
			fio: null,
			isEditPassword: false,
			isPasswordMasked: true,
			password: null,
	    }},
		methods: {
			unmaskPassword: function() {
				this.isPasswordMasked = false;
			},
			maskPassword: function() {
				this.isPasswordMasked = true;
			},
			saveNewPassword: function() {
				let password = this.password;
				if (password.length >= 8) {
					this.isEditPassword=false
					this.password = '';
					fetch('https://моидокументы.рф/mfc/ws/auth/updatePassword', {
						credentials: "same-origin",
						method: 'post',
						headers: {
							"Content-type": "application/json;charset=UTF-8"
						},
						body: '{password: "' + password + '"}'
					})
					.then((response) => {
						if(!response.ok) {
							app.$Message.error({
								message:'Неверный ответ сервера',
								duration: 3000
							});
							throw new Error('Неверный ответ сервера');
						}
						response.json().then(function(data) {
							app.$Message.success({
								message:'Успех!',
								duration: 3000
							});
							app.updateLogin();
						});
					})
					.catch((error) => {
						throw new Error(error);
					});
				}
			},
			cancelEditPassword: function() {
				this.isEditPassword=false
				this.password = '';
			},
			cancelEditFio: function() {
				this.isEditFio=false
				this.fio = this.currentUser.fio;
			},
			saveNewFio: function() {
				let fio = this.fio;
				this.isEditFio=false;
				fetch('https://моидокументы.рф/mfc/ws/auth/updateFio', {
					credentials: "same-origin",
					method: 'post',
					headers: {
						"Content-type": "application/json;charset=UTF-8"
					},
					body: '{fio: "' + fio + '"}'
				})
				.then((response) => {
					if(!response.ok) {
						app.$Message.error({
							message:'Неверный ответ сервера',
							duration: 3000
						});
						throw new Error('Неверный ответ сервера');
					}
					response.json().then(function(data) {
						app.$Message.success({
							message:'Успех!',
							duration: 3000
						});
						app.updateLogin();
					});
				})
				.catch((error) => {
					throw new Error(error);
				});
			},
		},
		created: function() {
			console.log(this.currentUser);
			this.fio = this.currentUser.fio;
		},
	};
</script>

<style>
	
</style>