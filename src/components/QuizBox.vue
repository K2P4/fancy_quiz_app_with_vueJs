<!-- @format -->

<template>
	<div
		class="shadow rounded-sm bg-secondary-subtle mt-2 grid col-sm-6 col-12 mx-auto p-5">
		<h3
			v-html="currentQuestion.question"
			class="text-center"></h3>

		<hr />

		<ul class="list-group">
			<li
				v-for="(answer, index) in randomAnswers"
				:key="index"
				v-html="answer"
				@click.prevent="selectedQuiz(index)"
				:class="[answerClass(index), 'text-center list-group-item']"></li>
		</ul>

		<div class="d-flex align-items-center mt-3 gap-1 justify-content-center">
			<button
				:disabled="selectedIndex === null || answered"
				@click="submitAnswers"
				class="btn btn-primary w-full px-5 py-2">
				Submit
			</button>

			<button
				:disabled="isLastQuestion"
				@click="NextQ"
				class="btn btn-success w-full px-5 py-2">
				Next
			</button>
		</div>
	</div>
</template>

<script setup>
	import _ from "lodash";
	import { computed, defineProps, reactive, ref, watch } from "vue";

	const props = defineProps({
		currentQuestion: {
			type: Object,
			require: true,
		},

		NextQ: {
			type: Function,
			require: true,
		},

		increment: {
			type: Function,
		},

		isLastQuestion: Boolean,
	});

	const selectedIndex = ref(null);
	const correctIndex = ref(null);
	const randomAnswers = ref([]);
	const answered = ref(false);

	const selectedQuiz = (index) => {
		selectedIndex.value = index;
	};

	watch(
		() => props.currentQuestion,
		() => {
			selectedIndex.value = null;
			answered.value = false;
			shuffleAnswers();
		},
		{ immediate: true },
	);

	function shuffleAnswers() {
		const answers = [
			...props.currentQuestion.incorrect_answers,
			props.currentQuestion.correct_answer,
		];
		randomAnswers.value = _.shuffle(answers);
		correctIndex.value = randomAnswers.value.indexOf(
			props.currentQuestion.correct_answer,
		);
	}

	const submitAnswers = () => {
		const isCorrect = selectedIndex.value === correctIndex.value;
		answered.value = true;
		props.increment(isCorrect);
	};

	function answerClass(index) {
		if (!answered.value && selectedIndex.value === index) return "selected";
		if (answered.value && correctIndex.value === index) return "correct";
		if (
			answered.value &&
			selectedIndex.value === index &&
			correctIndex.value !== index
		)
			return "incorrect";
		return "";
	}
</script>

<style scoped>
	.list-group {
		margin-bottom: 15px;
	}

	.list-group-item {
		background-color: #f3f3f3;
	}

	.list-group-item:hover {
		background: #f0f0f0;
		cursor: pointer;
	}

	.btn {
		margin: 0 5px;
	}

	.selected {
		background-color: lightblue;
	}

	.correct {
		background-color: lightgreen;
	}

	.incorrect {
		background-color: rgb(245, 81, 81);
	}
</style>
