<template>
	<modal title="Registration" @close="closeModal">
		<div slot="body">
			<form @submit.prevent="submit">
				<!-- email -->
				<div class="form-item" :class="{ errorInput: $v.email.$error }">
					<p class="errorText" v-if="!$v.email.required">Field is required!</p>
					<p class="errorText" v-if="!$v.email.email">Email is not correct!</p>
					<label>
						Email:
						<input
								v-model="email"
								:class="{ error: $v.email.$error  }"
								@change="$v.email.$touch()">
					</label>
				</div>
				<!-- password -->
				<div class="form-item" :class="{ errorInput: $v.password.$error }">
					<p class="errorText" v-if="!$v.password.required">Field is required!</p>
					<p class="errorText" v-if="!$v.password.minLength">Password must have at least
						{{$v.password.$params.minLength.min}}
						letters.</p>
					<p class="passwordErrorText" v-if="!passwordsAreSame">Passwords should be same!</p>
					<label>
						Password:
						<input
								type="password"
								v-model="password"
								:class="{ error: $v.password.$error  }"
								@change="$v.password.$touch()">
					</label>
				</div>
				<!-- password confirm -->
				<div class="form-item" :class="{ errorInput: $v.passwordConfirm.$error }">
					<p class="errorText" v-if="!$v.passwordConfirm.required">Field is required!</p>
					<p class="errorText" v-if="!$v.passwordConfirm.minLength">Password must have at least
						{{$v.passwordConfirm.$params.minLength.min}}
						letters.</p>
					<p class="passwordErrorText" v-if="!passwordsAreSame">Passwords should be same!</p>
					<label>
						Confirm password:
						<input
								type="password"
								v-model="passwordConfirm"
								:class="{ error: $v.passwordConfirm.$error  }"
								@change="$v.passwordConfirm.$touch()">
					</label>
				</div>
				<button class="btn btnPrimary">Submit</button>
			</form>
			<p class="text" @click="redirectToLogin">I already have an account</p>
		</div>
	</modal>
</template>

<script>
	import modal from '@/components/UI/Modal';
	import { required, minLength, email } from 'vuelidate/lib/validators';


	export default {
		components: {
			modal
		},
		data() {
			return {
				email: '',
				password: '',
				passwordConfirm: '',
				passwordsAreSame: true,
			}
		},
		methods: {
			closeModal() {
				this.email = '';
				this.password = '';
				this.passwordConfirm = '';
				this.passwordsAreSame = true;
				this.$v.$reset();
				this.$emit('close');
			},
			submit () {
				this.$v.$touch();
				if (!this.$v.$invalid) {
					if (this.passwordConfirm === this.password) {
						const user = {
							email: this.email,
							password: this.password,
							passwordConfirm: this.passwordConfirm
						};
						console.log(user);
						this.closeModal();
					} else {
						this.passwordsAreSame = false;
					}
				}
			},
			redirectToLogin() {
				this.closeModal();
				this.$emit('openLoginModal')
			}
		},
		validations: {
			email: {
				required,
				email,
			},
			password: {
				required,
				minLength: minLength(10)
			},
			passwordConfirm: {
				required,
				minLength: minLength(10)
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

	.passwordErrorText {
		margin-bottom: 8px;
		font-size: 13.4px;
		color: #de4040;
	}

	input.error {
		border-color: #de4040;
	}
</style>