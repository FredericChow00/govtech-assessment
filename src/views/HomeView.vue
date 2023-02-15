<template>
    <img alt="GovTech logo" src="../assets/logo.png">
    <h1>{{ title }}</h1>
    <div class="component">
        <date-time-input @getDateTime="handleDateTimeInput" />
    </div>
    <div class="component" v-if="selectedDateTime">
        <location-info :selectedDateTime="this.selectedDateTime" @getLocation="this.handleLocation" />
    </div>
    <div class="component" v-if="selectedLocation">
        <camera-photo :selectedLocation="this.selectedLocation" />  
        <weather-info :selectedDateTime="this.selectedDateTime" :selectedLocation="this.selectedLocation" @getWeather="this.handleWeather" />
    </div>
</template>

<script>
import DateTimeInput from '@/components/DateTimeInput.vue';
import LocationInfo from '@/components/LocationInfo.vue';
import CameraPhoto from '@/components/CameraPhoto.vue';
import WeatherInfo from '@/components/WeatherInfo.vue';

export default {
    name: 'HomeView',
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
            this.selectedLocation = undefined;
            this.cameraScreenshot = undefined;
            this.weather = undefined;
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

<style scoped>
h1 {
    border-bottom: 1px solid #ddd;
    display: inline-block;
    padding-bottom: 10px;
}
.component {
    margin: 10px;
}
</style>
