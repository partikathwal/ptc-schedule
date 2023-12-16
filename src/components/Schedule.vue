<template>
	<div class="h-screen text-center w-full">
		<table class="bg-white border-collapse whitespace-nowrap border border-gray-700">
			<tbody>
				<tr class="bg-sky-950 text-white font-bold">
					<td class="py-4 text-xl" colspan="6">Edison Hindi Congregation Public Talk Schedule</td>
				</tr>
				<template v-for="monthWeeks in months" :key="monthWeeks.month">
					<tr class="bg-sky-700 text-white font-bold">
						<td class="p-2" colspan="6">{{ monthWeeks[0].monthName }} {{ monthWeeks[0].year }}</td>
					</tr>
					<tr class="font-bold border-b border-gray-700">
						<td class="p-2 bg-sky-200">Date</td>
						<td class="p-2">Talk Title</td>
						<td class="p-2">Speaker</td>
						<td class="p-2">Chairman</td>
						<td class="p-2">Reader</td>
						<td class="p-2">Hospitality</td>
					</tr>
					<tr v-for="week in monthWeeks" :key="week.day" class="border-b border-gray-500">
						<td class="p-2 bg-sky-200 h-16">{{ week.monthName }} {{ week.day }}</td>
						<td class="p-2">
							<div v-if="week.type === 'stream'" class="font-bold">
								<div>Stream Talk</div>
							</div>
							<div v-else-if="week.type === 'memorial'" class="font-bold">
								<div>Memorial</div>
							</div>
							<div v-else class="flex flex-col">
								<div class="font-bold">{{ week.theme_upper || week.hindiTitle }}</div>
								<div>{{ week.theme_lower || week.title }}</div>
							</div>
						</td>
						<td class="p-2">
							<div v-if="week.type === 'stream'" class="font-bold">
								<div>JW Stream</div>
							</div>
							<div v-else class="flex flex-col">
								<div class="font-bold">{{ week.speaker }}</div>
								<div class="text-sm">{{ week.type === "co" ? "Circuit Overseer" : week.congregation }}</div>
							</div>
						</td>
						<td class="p-2">{{ week.chairman }}</td>
						<td class="p-2">{{ week.reader }}</td>
						<td class="p-2" :class="{'font-bold': week.hospitality}">{{ week.type === "stream" ? "(Stream)" : (week.type === "zoom") ? "(Zoom)" : (week.type === "local") ? "(Local)" : (week.type === "co") ? "(CO)" : (week.type === "memorial") ? "(Memorial)" : week.hospitality }}</td>
					</tr>
				</template>
			</tbody>
		</table>
	</div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps(["weeks"]);

const months = computed(() => {
	return Object.values(Object.groupBy(props.weeks, (week => week.month)));
})

</script>


<style>
@media print {
  body {
    zoom: 80%;
  }
}

</style>