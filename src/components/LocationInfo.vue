<template>
    <div>
      <label for="location-select">Select a location:</label>
      <select id="location-select" v-model="selectedLocation" @change="loadLocation">
        <option v-for="location in locations" :value="location">{{ location.name }}</option>
      </select>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    props:['selectedDateTime'],
    data() {
        return {
            selectedLocation: undefined,
            locations: [],
            weather: undefined
        }
    },
    methods: {
        async loadLocation() {
            try {
                const selectedDateTime = this.selectedDateTime;
                const response = await axios.get('https://api.data.gov.sg/v1/transport/traffic-images', {
                    params: {
                        date_time: selectedDateTime
                    }
                });
                const cameras = response.data.items[0].cameras;
                const locations = cameras.map(camera => ({
                    // will fill up the name and weather after using the Weather API
                    name: null,
                    coords: camera.location,
                    image: camera.image,
                    weather: null
                }));

                // Use reverse geocoding to get the location name
                await Promise.all(locations.map(async location => {
                    const response = await axios.get('https://api.data.gov.sg/v1/environment/2-hour-weather-forecast', {
                        params: {
                            date_time: selectedDateTime
                        },
                    });
                    location.name = response.data.area_metadata.find(area => area.label_location === location.coords).name;
                }));
                this.locations = locations;
            } catch(error) {
                console.log(error);
                alert("Error getting location!");
            }
        },
        emitLocation() {
            this.$emit(getLocation, this.selectedLocation);
        }
    }
}
</script>
    