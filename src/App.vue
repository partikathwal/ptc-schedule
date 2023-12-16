<template>

	<div class="no-print flex flex-col gap-2 w-64 p-4">
		<input class="border border-black p-2" type="text" v-model="monthsText" placeholder="months (e.g. 10,11,12)">
		<input class="border border-black p-2" type="text" v-model="yearText" placeholder="year">
		<button @click="go()" class="bg-blue-700 text-white p-2">Go</button>
	</div>

	<Schedule v-if="done" :weeks="weeks" />
</template>

<script setup>

import { ref } from "vue";
import Schedule from "./components/Schedule.vue";

const yearText = ref("");
const monthsText = ref("");
const weeks = ref([]);
const done = ref(false);

async function go(){
	weeks.value = await getData(parseInt(yearText.value), monthsText.value);
	done.value = true;
}

async function getData(year, months){
	const url = `https://script.google.com/macros/s/AKfycbyKbbAEF3TMoujRp91gb7uxqTSYXHalt5mV5QRf5Lj8--j-gPRGXLjGmv8JgNnSO1OY/exec?year=${year}&months=${months}`;
	const data = await fetch(url).then(res => res.json());
	data.forEach(week => week.monthName = monthNames[parseInt(week.month) - 1]);
	return data;
}

const monthNames = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

</script>

<style>

@media print {
	.no-print {
		display: none;
	}
}
</style>