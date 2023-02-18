<template>
	<div class="datetimeinput container">
		<div class="row">
			<div class="col-6">
				<label for="date-select" class="form-label">Select a date:</label>
				<select id="date-select" v-model="selectedDate" class="form-select">
					<option v-for="dateInterval in dateIntervals" v-bind:key="dateInterval" :value="dateInterval">{{ dateInterval }}</option>
				</select>
			</div>
			<div class="col-6">
				<label for="time-select" class="form-label">Select a time:</label>
				<select id="time-select" v-model="selectedTime" @change="emitDateTime" class="form-select">
					<option v-for="timeInterval in timeIntervals" v-bind:key="timeInterval" :value="timeInterval">{{ timeInterval }}</option>
				</select>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'DateTimeInput',
	data() {
		return {
			selectedDate: undefined,
			selectedTime: undefined,
		}
	},
	computed: {
		// up to 3 months ago and 1 day in advance
		dateIntervals() {
			const intervals = [];
			for (let i = -2; i < 120; i++) {
				const start = new Date();
				start.setDate(start.getDate() - i);
				start.setHours(0, 0, 0, 0);
				let isoString = start.toISOString();
				intervals.push(`${isoString.substring(0, isoString.length - 14)}`);
			}
			return intervals;
		},
		// 30 min time intervals (since weather forecast only in 30 min intervals)
		timeIntervals() {
			const times = [];
			for (let i = 0; i < 24; i++) {
				const hour = i < 10 ? `0${i}` : i;
				for (let j = 0; j < 60; j += 30) {
					const minute = j < 10 ? `0${j}` : j;
					times.push(`${hour}:${minute}`);
				}
			}
			return times;
		}
	},
	methods: {
		emitDateTime() {
			// put in ISOString format
			const dateTime = `${this.selectedDate}T${this.selectedTime}:00`;
			this.$emit('getDateTime', dateTime);		}
	},
};
</script>

<style scoped>
.datetimeinput {
  background: lightgrey;
  padding: 10px;
  border-radius: 10px;
}
</style>
