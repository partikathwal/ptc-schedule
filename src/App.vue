<template>

	<form @submit.prevent="go()" class="no-print flex flex-col gap-2 w-64 p-4">
		<input ref="monthInput" class="border border-black p-2" type="text" v-model="monthsText" placeholder="months (e.g. 10,11,12)">
		<input class="border border-black p-2" type="text" v-model="yearText" placeholder="year">
		<button @click="go()" class="bg-blue-700 text-white p-2">Go</button>
	</form>

	<Schedule v-if="done" :weeks="weeks" />

	<Loader v-if="isLoading" class="no-print"></Loader>
	
</template>

<script setup>

import { onMounted, ref } from "vue";
import Schedule from "./components/Schedule.vue";
import Loader from "./components/Loader.vue";

const monthInput = ref();
const isLoading = ref(false);

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

	isLoading.value = true;
	const data = await fetch(url).then(res => res.json());
	isLoading.value = false;

	data.forEach(week => week.monthName = monthNames[parseInt(week.month) - 1]);
	document.title = `${monthNames[months[0] - 1].slice(0, 3)} - ${monthNames[months[months.length - 1] - 1].slice(0, 3)} ${year} PT Schedule`
	return data;
}

const monthNames = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

onMounted(() => {
	yearText.value = new Date().getFullYear();
	monthInput.value.focus();
})

</script>

<style>

@media print {
	.no-print {
		display: none;
	}
}
</style>