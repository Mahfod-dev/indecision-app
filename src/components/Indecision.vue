<template>
	<img v-if="image" :src="image" alt="bg" />
	<div class="bg-dark"></div>

	<div class="indecision-container">
		<input
			type="text"
			placeholder="Posez moi une question"
			v-model="question"
		/>

		<p>N'oubliez pas de terminer par un point d'interrogation (?)</p>
		<div v-if="isValidQuestion">
			<h2>{{ question }}</h2>
			<h1>{{ answer === 'yes' ? 'Oui!' : 'Non!' }}</h1>
			<!-- Si!: YES -->
			<!-- No!: No -->
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				question: null,
				answer: null,
				image: null,
				isValidQuestion: false,
			};
		},
		methods: {
			async getAnswer() {
				this.anwer = 'Alors......';
				const { answer, image } = await fetch('https://yesno.wtf/api').then(
					(res) => res.json()
				);
				this.answer = answer;
				this.image = image;
			},
		},

		watch: {
			question(value, oldvalue) {
				this.isValidQuestion = false;

				if (!value.includes('?')) return;

				this.isValidQuestion = true;

				this.getAnswer();
			},
		},
	};
</script>

<style scoped>
	img .bg-dark {
		height: 100vh;
		left: 0px;
		max-height: 100%;
		max-width: 100%;
		position: fixed;
		top: 0px;
		width: 100vw;
	}

	.bg-dark {
		background-color: rgba(0, 0, 0, 0.4);
	}

	.indecision-container {
		position: relative;
		z-index: 99;
	}

	input {
		width: 250px;
		padding: 10px 15px;
		border-radius: 5px;
		border: none;
	}
	input:focus {
		outline: none;
	}

	p {
		color: white;
		font-size: 20px;
		margin-top: 0px;
	}

	h1,
	h2 {
		color: white;
	}

	h2 {
		margin-top: 150px;
	}
</style>
