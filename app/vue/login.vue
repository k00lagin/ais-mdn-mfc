<template>
	<div class="login">
		<form action="https://моидокументы.рф/mfc/ws/auth/login" method="post" @submit.prevent="submitLogin">
			<label>
				Логин:
				<at-input v-model="username" placeholder=""></at-input>
			</label>
			<label>
				Пароль:
				<at-input v-model="password" placeholder="" type="password"></at-input>
			</label>
			<at-checkbox v-model="rememberLogin">Запомнить меня</at-checkbox>
			<at-button type="primary" @click="handleLoginClick">Войти</at-button>
		</form>
		<at-button @click="showLoginWindow">Войти через ЕСИА</at-button>
	</div>
</template>
<script>
	module.exports = {
		data: function () {
			return {
				username: "",
				password: "",
				rememberLogin: true
			};
		},
		methods: {
			showLoginWindow: function () {
				let login = nw.Window.open(
					"app/blank.html",
					{
						width: 1024,
						height: 768,
					},
					(win) => {
						win.eval(null, `window.location.href = "http://xn--d1achjhdicc8bh4h.xn--p1ai/mfc/esia/Login";`);
						let checkAuth = setInterval(() => {
							if (win.window.location.host === 'xn--d1achjhdicc8bh4h.xn--p1ai' && window.location.href !== 'http://xn--d1achjhdicc8bh4h.xn--p1ai/mfc/esia/Login') {
								win.close();
								clearTimeout(checkAuth);
								this.$emit('auth');
							}
						}, 100);
					}
				);
			},
			handleLoginClick: function () {
				if (this.rememberLogin) {
					localStorage.setItem("rememberLogin", this.rememberLogin);
					localStorage.setItem("username", this.username);
					localStorage.setItem("password", this.password);
				} else {
					localStorage.removeItem("rememberLogin");
					localStorage.removeItem("username");
					localStorage.removeItem("password");
				}
				this.login();
			},
			login: function () {
				fetch("https://моидокументы.рф/mfc/ws/auth/login", {
					credentials: "same-origin",
					method: "post",
					headers: {
						"Content-type": "application/x-www-form-urlencoded; charset=UTF-8"
					},
					body: "username=" + this.username + "&password=" + this.password
				})
					.then(response => {
						if (!response.ok) {
							throw new Error("Network response was not ok");
						}
						response.json().then(function(data) {
							app.updateLogin();
						});
					})
					.catch(error => {
						throw new Error(error);
					});
			}
		}
	};
</script>
<style scoped>
	.login {
		width: 200px;
		margin: 0 auto;
		display: flex;
		flex-flow: column nowrap;
		justify-content: center;
		align-items: stretch;
		height: 100%;
	}

	form {
		display: flex;
		flex-flow: column nowrap;
		justify-content: center;
		align-items: stretch;
	}

	form label,
	form button {
		margin-bottom: 8px;
	}

	form button {
		margin-bottom: 16px;
	}
</style>
