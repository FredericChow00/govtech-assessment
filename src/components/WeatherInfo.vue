<template>
    <h2>Weather info for {{ selectedLocation.name }}</h2>
    <p>{{ loadWeather.temperature }}</p>
</template>

<script>
import axios from 'axios';

export default {
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
        emitWeather() {
            this.$emit("getWeather", this.weather);
        }
    }
}
</script>
