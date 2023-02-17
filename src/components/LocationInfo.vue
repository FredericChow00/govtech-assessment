<template>
    <div class="locationinfo">
        <div class="form-group">
            <label for="location-select" class="form-label">Select a location:</label>
            <select class="form-select" v-if="locations.length > 0" id="location-select" v-model="selectedLocation" @change="emitLocation">
                <option v-for="location in locations" v-bind:key="location" :value="location">{{ location.name }}</option>
            </select>
            <p v-else >Loading locations available...</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'LocationInfo',
    props:['selectedDateTime'],
    data() {
        return {
            selectedLocation: undefined,
            locations: []
        }
    },
    computed: {
        getLocations() {
            return this.loadLocation();
        }
    },
    methods: {
        truncateCoords(coordsObj) {
            try {
                coordsObj.latitude = Number(coordsObj.latitude.toFixed(2));
                coordsObj.longitude = Number(coordsObj.longitude.toFixed(2));
                return coordsObj;
            } catch (error) {
                console.log(error);
            }
        },
        isSameCoords(coord1, coord2) {
            coord1 = this.truncateCoords(coord1);
            coord2 = this.truncateCoords(coord2);
            if (coord1.longitude === coord2.longitude && coord1.latitude === coord2.latitude) {
                return true;
            } else {
                return false;
            }
        },
        async loadLocation() {
            let locations;
            try {
                const selectedDateTime = this.selectedDateTime;
                const response = await axios.get('https://api.data.gov.sg/v1/transport/traffic-images', {
                    params: {
                        date_time: selectedDateTime
                    }
                });
                const cameras = response.data.items[0].cameras;
                locations = cameras.map(camera => ({
                    // will fill up the name and weather after using the Weather API
                    name: undefined,
                    coords: camera.location,
                    image: camera.image,
                    weather: undefined
                }));
                console.log(locations);

                // Use reverse geocoding to get the location name
                await Promise.all(locations.map(async location => {
                    const response = await axios.get('https://api.data.gov.sg/v1/environment/2-hour-weather-forecast', {
                        params: {
                            date_time: selectedDateTime
                        },
                    });
                    //console.log(location.name);
                    let matchedArea = response.data.area_metadata.filter(area => this.isSameCoords(area.label_location, location.coords))
                    console.log(matchedArea.length);
                    console.assert(matchedArea.length <= 1, "MORE THAN 1 AREA MATCHED");
                    if (matchedArea.length == 0) {
                        location.name = undefined;
                    } else {
                        location.name = matchedArea[0].name;
                        console.log(`new ${location.name}`);
                    }
                }));
                // remove locations with no match and sort by location name
                locations = Array.from(new Set(locations.filter(location => location.name))).sort((l1, l2) => l1.name.localeCompare(l2.name));
                //console.log(locations);
            } catch(error) {
                console.log(error);
                alert("Error getting location info!");
            }
            console.log(locations);
            return locations;
        },
        emitLocation() {
            this.$emit("getLocation", this.selectedLocation);
        }
    },
    async created() {
        this.locations = await this.loadLocation();
    },
    watch: {
        selectedDateTime: async function() {
            this.locations = [];
            this.locations = await this.loadLocation();
        }
    }
};
</script>
    
<style scoped>
.locationinfo {
	background: lightgrey;
	padding: 10px;
	border-radius: 10px;
}
</style>