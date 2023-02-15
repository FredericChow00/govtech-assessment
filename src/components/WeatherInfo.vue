<template>
    <div class="weatherinfo">
        <h2>Weather info for {{ selectedLocation.name }}</h2>
        <p>{{ loadWeather.temperature }}</p>
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
        emitWeather() {
            this.$emit("getWeather", this.weather);
        }
    },
};
</script>

<style scoped>
.weatherinfo {
    right: 50px;
    width: 25%;
    height: 30%;
    padding: 20px;
    margin: 100px auto;
    background: lightgray;
    border-radius: 10px;
}
</style>
