<template>
	<modal title="Login" @close="closeModal">
		<div slot="body">
			<form @submit.prevent="submit">
				<div class="form-item" :class="{ errorInput: $v.login.$error }">
					<label for="login">Login</label>
					<p class="errorText" v-if="!$v.login.required">Field is required!</p>
					<input v-model="login" :class="{ error: $v.login.$error }" id="login">
				</div>
				<div class="form-item" :class="{ errorInput: $v.password.$error }">
					<label for="password">Password</label>
					<p class="errorText" v-if="!$v.password.required">Field is required!</p>
					<input v-model="password" :class="{ error: $v.password.$error }" type="password" id="password">
					<button class="btn btnPrimary">Submit</button>
				</div>
			</form>
			<p class="text" @click="redirectToRegistration">I need an account</p>
		</div>
	</modal>
</template>

<script>
	import modal from '@/components/UI/Modal';
	import { required } from 'vuelidate/src/validators';

	export default {
		components: {
			modal
		},
		data() {
			return {
				login: '',
				password: ''
			}
		},
		methods: {
			submit() {
				this.$v.$touch();
				if (!this.$v.$invalid) {
					const user = {
						login: this.login,
						password: this.password,
					};
					console.log(user);
					this.closeModal()
				}
			},
			closeModal() {
				this.login = '';
				this.password = '';
				this.$v.$reset();
				this.$emit('close');
			},
			redirectToRegistration() {
				this.closeModal();
				this.$emit('openRegistrationModal')
			}
		},
		validations: {
			login: {
				required,
			},
			password: {
				required
			}
		}
	}
</script>

<style lang="scss">
	.form-item .errorText {
		display: none;
		margin-bottom: 8px;
		font-size: 13.4px;
		color: #de4040;
	}

	.form-item {
		&.errorInput .errorText {
			display: block;
		}
	}

	.text {
		margin-top: 20px;
		font-size: 15px;
		color: grey;
		cursor: pointer;

		&:hover {
			text-decoration: underline;
		}
	}

	input.error {
		border-color: #de4040;
	}
</style>