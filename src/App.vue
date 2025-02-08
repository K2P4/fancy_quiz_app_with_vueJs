<!-- @format -->

<script setup>
	import { computed, onMounted, reactive, ref } from "vue";
	import NavHeader from "./components/NavHeader.vue";
	import QuizBox from "./components/QuizBox.vue";

	const questions = ref([]);
	const correctTotal = ref(0);
	const numTotal = ref(0);
	const index = ref(0);

	const isLastQuestion = computed(
		() => index.value === questions.value.length - 1,
	);

	const NextQ = () => {
		if (index.value < questions.value.length - 1) {
			index.value++;
		}
	};

	const percentageScore = computed(() => {
		return numTotal.value > 0
			? ((correctTotal.value / numTotal.value) * 100).toFixed(2)
			: 0;
	});

	function increment(isCorrect) {
		if (isCorrect) correctTotal.value++;
		numTotal.value++;
	}

	onMounted(() => {
		fetch("https://opentdb.com/api.php?amount=10&type=multiple", {
			method: "get",
		})
			.then((response) => {
				return response.json();
			})
			.then((jsonData) => {
				questions.value = jsonData.results;
			});
	});
</script>

<template>
	<div>
		<NavHeader
			:correctTotal="correctTotal"
			:numTotal="numTotal" />
		<QuizBox
			v-if="questions.length"
			:currentQuestion="questions[index]"
			:isLastQuestion="isLastQuestion"
			:NextQ="NextQ"
			:increment="increment" />

		<div
			v-if="isLastQuestion"
			class="mt-2 text-center">
			<h3 class="fw-medium">
				Your Final Score: {{ correctTotal }}/{{ numTotal }} ({{
					percentageScore
				}}%)
			</h3>
		</div>
	</div>
</template>
