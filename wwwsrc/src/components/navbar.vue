<template>
	<nav class="navbar navbar-expand-lg navbar-light bg-primary">
		<router-link class="navbar-brand" :to="{ name: 'home' }">Pic Vault</router-link>
		<button
			class="navbar-toggler"
			type="button"
			data-toggle="collapse"
			data-target="#navbarText"
			aria-controls="navbarText"
			aria-expanded="false"
			aria-label="Toggle navigation"
		>
			<span class="navbar-toggler-icon"></span>
		</button>
		<div class="collapse navbar-collapse" id="navbarText">
			<ul class="navbar-nav mr-auto">
				<li class="nav-item" :class="{ active: $route.name == 'home' }">
					<router-link :to="{ name: 'home' }" class="nav-link">Home</router-link>
				</li>
				<li
					class="nav-item"
					v-if="$auth.isAuthenticated"
					:class="{ active: $route.name == 'dashboard' }"
				>
					<router-link class="nav-link" :to="{ name: 'dashboard' }">Dashboard</router-link>
				</li>
			</ul>
			<span class="navbar-text">
				<button class="btn btn-success" @click="login" v-if="!$auth.isAuthenticated">Login/Register</button>
				<button class="btn btn-danger" @click="logout" v-else>Logout</button>
			</span>
		</div>
	</nav>
</template>

<script>
import axios from "axios";

let _api = axios.create({
	baseURL: "https://localhost:5001",
	withCredentials: true
});
export default {
	name: "Navbar",
	methods: {
		async login() {
			await this.$auth.loginWithPopup();
			await this.$auth.getUserData();
			this.$store.dispatch("setBearer", this.$auth.bearer);
		},
		async logout() {
			await this.$auth.logout();
			this.$store.dispatch("resetBearer");
			// if (this.$route.name != "keep") {
			// 	this.$router.push({ name: "home" });
			// }
		}
	}
};
</script>

<style></style>
