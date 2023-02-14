<template>
    <div>
        <h2>Weather info for {{ selectedLocation.name }}</h2>
        <p>{{ weather.temperature }}</p>
    </div>
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
    computed: {
        async loadWeather() {
            try {
                const response = await axios.get('https://api.data.gov.sg/v1/environment/2-hour-weather-forecast', {
                    params: {
                        date_time: selectedDateTime
                    },
                });
                const weather = response.data.items[0].forecasts.find(forecast =>
                        forecast.area === this.selectedLocation.name);
                this.weather = weather;
            } catch (error) {
                console.error(error);
                alert('Error getting weather!');
            }
        },
    },
    methods: {
        emitWeather() {
            this.$emit(getWeather, this.weather);
        }
    }
}
</script>
