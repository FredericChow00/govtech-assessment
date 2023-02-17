<template>
    <div class="container">
        <div class="row align-items-center">
            <div class="col-4 mx-auto img-fluid custom-size">
                <img alt="GovTech logo" src="../assets/logo.png">
            </div>
            <div class="col-12 col-md-8 mt-3 text-center">
                <h1>{{ title }}</h1>
            </div>
        </div>
        <div class="col-12 col-md-8 ">
            <date-time-input @getDateTime="handleDateTimeInput" />
        </div>
        <div class="row d-flex">
            <div class="col-12 col-md-8 align-items-stretch mt-3">
                <div v-if="selectedDateTime">
                    <location-info :selectedDateTime="selectedDateTime" @getLocation="handleLocation" />
                </div>
            </div>  
            <div class="col-12 col-md-4 align-items-stretch mt-3">
                <div v-if="selectedLocation && selectedDateTime">
                    <weather-info :selectedDateTime="selectedDateTime" :selectedLocation="selectedLocation" />
                </div>
            </div>
        </div>
        <div class="col-12 col-md-8 mt-3">
            <div v-if="selectedLocation">
                <camera-photo :selectedLocation="selectedLocation" />
            </div>
        </div>
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
            title: "Traffic Camera and Weather Forecasts",
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
    }
}
</script>

<style scoped>
.custom-size {
    width: 200px;
    height: 200px;
}
</style>

