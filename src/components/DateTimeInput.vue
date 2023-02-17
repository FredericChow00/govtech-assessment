<template>
	<div class="datetimeinput container">
		<div class="row">
			<div class="col-6">
				<label for="date-select" class="form-label">Select a date:</label>
				<select id="date-select" v-model="selectedDateTime" @change="emitDateTime" class="form-select">
					<option v-for="dateInterval in intervals" v-bind:key="dateInterval" :value="dateInterval">{{ dateInterval }}</option>
				</select>
			</div>
			<div class="col-6">
				<label for="time-select" class="form-label">Select a time:</label>
				<select id="time-select" v-model="selectedDateTime" @change="emitDateTime" class="form-select">
					<option v-for="timeInterval in intervals" v-bind:key="timeInterval" :value="timeInterval">{{ timeInterval }}</option>
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
			selectedDateTime: undefined,
		}
	},
	// allows for user to choose a date up the 4 days ago 
	computed: {
		intervals() {
			const intervals = [];
			for (let i = 4; i >= 0; i--) {
				const start = new Date();
				start.setDate(start.getDate() - i);
				start.setHours(0, 0, 0, 0);
				const end = new Date(start);
				end.setDate(end.getDate() + 1);
				let isoString = start.toISOString();
				intervals.push(`${isoString.substring(0, isoString.length - 5)}`);
			}
			return intervals;
		}
	},
	methods: {
		emitDateTime() {
			this.$emit("getDateTime", this.selectedDateTime);
		}
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
