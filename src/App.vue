<script setup>
import TheQuiz from "./components/TheQuiz.vue";
</script>

<template>
	<main class="container">
		<TheQuiz />
	</main>
</template>

<style scoped>
* {
	margin: 0;
	padding: 0;
	font-family: "Montserrat";
}

*,
::after,
::before {
	box-sizing: border-box;
}

html {
	scroll-behavior: smooth;
}

.container,
.container-fluid {
	width: 100%;
	padding: 0 0.75rem;
	margin-right: auto;
	margin-left: auto;
}

@media (min-width: 576px) {
	.container {
		max-width: 540px;
	}
}
@media (min-width: 768px) {
	.container {
		max-width: 720px;
	}
}
@media (min-width: 992px) {
	.container {
		max-width: 960px;
	}
}
@media (min-width: 1200px) {
	.container {
		max-width: 1140px;
	}
}
@media (min-width: 1400px) {
	.container {
		max-width: 1320px;
	}
}
</style>
