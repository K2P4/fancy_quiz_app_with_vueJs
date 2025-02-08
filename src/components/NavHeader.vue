<!-- @format -->

<template>
	<ul class="nav py-2 border-b border">
		<li class="nav-item">
			<a
				class="nav-link text-secondary"
				aria-current="page"
				href="/"
				><h5 class="fw-bold">Quiz App</h5></a
			>
		</li>
		<li class="nav-item">
			<a
				class="nav-link text-secondary"
				href=""
				><h5>Counter : {{ correctTotal }}/{{ numTotal }}</h5></a
			>
		</li>
	</ul>
</template>

<script setup>
	defineProps({
		correctTotal: Number,
		numTotal: Number,
	});
</script>
