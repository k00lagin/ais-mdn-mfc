<template>
	<div class="login">
		<form action="https://моидокументы.рф/mfc/ws/auth/login" method="post" @submit.prevent="submitLogin">
			<label>
				Логин:
				<at-input v-model="username" placeholder=""></at-input>
			</label>
			<label>
				Пароль:
				<at-input v-model="password" placeholder=""></at-input>
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
					"https://xn--d1achjhdicc8bh4h.xn--p1ai/mfc/esia/Login",
					{},
					win => {
						console.log(win);
					}
				);
				console.log(login);
			},
			handleLoginClick: function () {
				if (rememberLogin) {
					localStorage.setItem("rememberLogin", rememberLogin);
					localStorage.setItem("username", username);
					localStorage.setItem("password", password);
				} else {
					localStorage.removeItem("rememberLogin");
					localStorage.removeItem("username");
					localStorage.removeItem("password");
				}
				login();
			},
			login: function () {
				fetch("https://моидокументы.рф/mfc/ws/auth/login", {
					credentials: "same-origin",
					method: "post",
					headers: {
						"Content-type": "application/x-www-form-urlencoded; charset=UTF-8"
					},
					body: "username=" + username + "&password=" + password
				})
					.then(response => {
						if (!response.ok) {
							throw new Error("Network response was not ok");
						}
						//response.json().then(function(data) {
						//  app.updateLogin();
						// });
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