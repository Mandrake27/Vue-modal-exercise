<template>
	<div class="wrapper">
		<div class="wrapper-content">
			<section>
				<div class="container">
					<!-- first modal -->
					<button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal</button>
					<modals v-show="modalFirst" title="First modal" @close="modalFirst = false">
						<div slot="body">
							<p>Text</p>
							<button class="btn btnPrimary" @click="modalFirst = !modalFirst">Close modal</button>
						</div>
					</modals>

					<!-- second modal -->
					<button
							class="btn btnPrimary"
							@click="modalSecond.show = !modalSecond.show"
					>Show modal with form
					</button>
					<modals v-show="modalSecond.show" title="Modal with form" @close="modalSecond.show = false">
						<div slot="body">
							<form @submit.prevent="submitSecondForm">
								<label>
									Name:
									<input type="text" required v-model="modalSecond.name">
								</label>
								<label>
									Email
									<input type="email" required v-model="modalSecond.email">
								</label>
								<button class="btn btnPrimary">Submit</button>
							</form>
						</div>
					</modals>
					<!-- form + validate -->
					<button
							class="btn btnPrimary"
							@click="modalValidate = !modalValidate"
					>
						Show modal with form + validate
					</button>
					<modalValidate v-show="modalValidate" @close="modalValidate = false"/>
				</div>

				<br>
				<!-- login modal -->
				<button class="btn btnPrimary" @click="loginModal = !loginModal">
					Show login modal
				</button>
				<loginModal v-show="loginModal" @close="loginModal = false" @openRegistrationModal="registrationModal = true" />

				<!-- registration modal -->
				<button class="btn btnPrimary" @click="registrationModal = !registrationModal">
					Show registration modal
				</button>
				<registration-modal v-show="registrationModal" @openLoginModal="loginModal = true" @close="registrationModal = false" />
			</section>
		</div>
	</div>
</template>

<script>
	import modals from "@/components/UI/Modal.vue";
	import modalValidate from '@/components/ModalValidate';
	import loginModal from '@/components/LoginModal';
	import registrationModal from '@/components/RegistrationModal';

	export default {
		components: {
			modals,
			modalValidate,
			loginModal,
			registrationModal
		},
		data() {
			return {
				modalFirst: false,
				modalSecond: {
					show: false,
					name: '',
					email: '',
				},
				modalValidate: false,
				loginModal: false,
				registrationModal: false,
			};
		},
		methods: {
			submitSecondForm() {
				console.log({
					name: this.modalSecond.name,
					email: this.modalSecond.email
				});
				this.modalSecond = {
					name: '',
					email: '',
					show: false
				}
			}
		}
	};
</script>
