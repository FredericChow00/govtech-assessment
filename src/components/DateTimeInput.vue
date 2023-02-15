<template>
	<div class="datetime">
		<label for="date-select">Select a date:</label>
		<select id="date-select" v-model="selectedDateTime" @change="emitDateTime">
		<option v-for="dateInterval in dateIntervals" v-bind:key="dateInterval" :value="dateInterval">{{ dateInterval }}</option>
		</select>
		<label for="time-select">Select a time:</label>
		<select id="time-select" v-model="selectedDateTime" @change="emitDateTime">
		<option v-for="timeInterval in timeIntervals" v-bind:key="timeInterval" :value="timeInterval">{{ timeInterval }}</option>
		</select>
	</div>
</template>

<script>
export default {
	data() {
		return {
			selectedDateTime: undefined,
		}
	},
	// allows for user to choose a date up the 4 days ago 
	computed: {
		dateIntervals() {
			const intervals = [];
			const currentDate = new Date();
			const earliestDate = new Date(currentDate - 4);
			for (let i = 0; i < 4; i++) {
			earliestDate.setDate(earliestDate.getDate() + i);
			intervals.push(`${earliestDate.toISOString()}`);
			}
			return intervals;
		},
		timeIntervals() {
			const intervals = [];
			for (let i = 0; i < 23; i++) {
			const start = new Date(this.selectedDateTime);
			start.setHours(i, 0, 0, 0);
			intervals.push(`${start.toISOString()}`);
			}
			return intervals;
		}
	},
	methods: {
		emitDateTime() {
			this.$emit("getDateTime", this.selectedDateTime);
		}
	}
}
</script>

<style>
	.datetime {
		width: 200px;
		padding: 20px;
		margin: 100px auto;
		background: grey;
		border-radius: 10px;
	}
	.datetime label {
		display: block;
	}
</style>
