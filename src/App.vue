<template>
	<img alt="GovTech logo" src="./assets/logo.png">
	<h1>{{ title }}</h1>
	<DateTimeInput @getDateTime="handleDateTimeInput" />
	<LocationInfo :selectedDateTime="this.selectedDateTime" @getLocation="this.handleLocation" />
	<CameraPhoto :selectedLocation="this.selectedLocation" />
	<WeatherInfo :selectedDateTime="this.selectedDateTime" :selectedLocation="this.selectedLocation" @getWeather="this.handleWeather" />
</template>

<script>
import DateTimeInput from './components/DateTimeInput.vue';
import LocationInfo from './components/LocationInfo.vue';
import CameraPhoto from './components/CameraPhoto.vue';
import WeatherInfo from './components/WeatherInfo.vue';

export default {
	name: 'App',
	components: {
		DateTimeInput,
		LocationInfo,
		CameraPhoto,
		WeatherInfo
	},
	data() {
		return {
			title: "Traffic and Weather Cameras",
			selectedDateTime: undefined,
			selectedLocation: undefined,
			cameraScreenshot: undefined,
			weather: undefined
		}
	},
	methods: {
		handleDateTimeInput(dateTime) {
			this.selectedDateTime = dateTime;
		},
		handleLocation(location) {
			this.selectedLocation = location;
			this.cameraScreenshot = this.selectedLocation.image;
		},
		handleWeather(weather) {
			this.weather = weather;
		}
	}
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}

@import'~bootstrap/dist/css/bootstrap.css'
</style>
