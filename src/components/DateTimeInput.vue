<template>
	<div class="datetimeinput">
		<div class="grid-container">
			<label for="date-select">Select a date:</label>
			<label for="time-select">Select a time:</label>
			<select id="date-select" v-model="selectedDateTime" @change="emitDateTime">
				<option v-for="dateInterval in intervals" v-bind:key="dateInterval" :value="dateInterval">{{ dateInterval }}</option>
			</select>
			<select id="time-select" v-model="selectedDateTime" @change="emitDateTime">
				<option v-for="timeInterval in intervals" v-bind:key="timeInterval" :value="timeInterval">{{ timeInterval }}</option>
			</select>
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
	top: 250px;
	left: 20px;
    width: 65%;
	background: lightgrey;
	display: inline-block;
	padding: 20px;
	margin: 10px auto;
	border-radius: 10px;
}

.grid-container {
	display: grid;
	grid-template-columns: 1fr 1fr;
	grid-gap: 10px;
}

label {
	align-self: center;
}

select {
	width: 100%;
}
</style>
