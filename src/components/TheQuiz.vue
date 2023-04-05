<script>
import IconPlay from "../components/icons/IconPlay.vue";
import { shuffle, range, random } from "lodash-es";
import QuizStore from "../quiz_store";

export default {
	data() {
		return {
			currentNumber: null,
			score: 0,
			numbers: range(1, QuizStore.MAX + 1),
		};
	},
	mounted() {
		this.generateNumber();
	},
	methods: {
		generateNumber() {
			this.currentNumber = random(1, QuizStore.MAX);
		},
		checkAnswer(num) {
			if (num === this.currentNumber) {
				this.incrementScore();
				this.shuffle();
				this.generateNumber();
				this.playAudio(this.currentNumber);
			} else {
				this.playAudio("wrong-answer");
				this.decrementScore();

				setTimeout(async () => {
					await this.playAudio(this.currentNumber);
				}, 700);
			}
		},
		playAudio(path) {
			let audio = new Audio(`./sounds/${path}.mp3`);
			audio.play();
		},
		incrementScore() {
			this.score++;
		},
		decrementScore() {
			if (this.score >= 1) {
				this.score--;
			}
		},
		shuffle() {
			this.numbers = shuffle(this.numbers);
		},
	},
	computed: {
		graphicalScore() {
			return "💎".repeat(this.score);
		},
	},
	components: {
		IconPlay,
	},
};
</script>

<template>
	<div class="quiz">
		<button class="quiz__play-btn" @click="playAudio(currentNumber)">
			<IconPlay />
		</button>

		<TransitionGroup tag="div" name="fade" class="quiz__grid">
			<div
				class="quiz__item"
				v-for="item in numbers"
				:key="item"
				@click="checkAnswer(item)"
			>
				{{ item }}
			</div>
		</TransitionGroup>

		<span class="quiz__score">
			{{ graphicalScore }}
		</span>
	</div>
</template>

<style scoped lang="scss">
.quiz {
	text-align: center;

	&__grid {
		display: grid;
		gap: 1.75rem;
		grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));

		@media (max-width: 575.98px) {
			grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
		}
	}

	&__play-btn {
		all: unset;
		cursor: pointer;
		margin-bottom: 3rem;
	}

	&__score {
		margin-top: 3rem;
		font-size: 3rem;
	}

	&__item {
		width: 200px;
		height: 200px;
		// width: auto;
		// height: auto;
		border-radius: 0.5rem;
		border: 1px solid green;
		font-size: 3rem;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		align-content: center;
		transition: border-color 0.2s;

		&:hover {
			cursor: pointer;
			border-color: red;
		}
	}

	.fade-move,
	.fade-enter-active,
	.fade-leave-active {
		transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
	}

	/* 2. declare enter from and leave to state */
	.fade-enter-from,
	.fade-leave-to {
		opacity: 0;
		transform: scaleY(0.01) translate(30px, 0);
	}

	/* 3. ensure leaving items are taken out of layout flow so that moving
      animations can be calculated correctly. */
	.fade-leave-active {
		position: absolute;
	}
}
</style>
