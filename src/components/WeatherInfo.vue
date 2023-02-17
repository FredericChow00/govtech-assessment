<template>
    <div class="weatherinfo p-3 rounded">
      <h4>Weather forecast for {{ selectedLocation.name }}</h4>
      <p v-if="this.weather">{{ this.weather.forecast }}</p>
      <p v-else>Loading weather forecast...</p>
    </div>
  </template>

<script>
import axios from 'axios';

export default {
    name: 'WeatherInfo',
    props:['selectedDateTime', 'selectedLocation'],
    data() {
        return {
            weather: undefined
        }
    },
    created() {
        this.loadWeather();
    },
    methods: {
        async loadWeather() {
            try {
                const selectedDateTime = this.selectedDateTime;
                const response = await axios.get('https://api.data.gov.sg/v1/environment/2-hour-weather-forecast', {
                    params: {
                        date_time: selectedDateTime
                    },
                });
                const loadedWeather = response.data.items[0].forecasts.find(forecast =>
                        forecast.area === this.selectedLocation.name);
                this.weather = loadedWeather;
            } catch (error) {
                console.error(error);
                alert('Error getting weather!');
            }
        },
    },
};
</script>

<style scoped>
.weatherinfo {
	background: lightgrey;
    border-radius: 10px;
}
</style>
