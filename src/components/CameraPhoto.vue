<template>
    <h2>Traffic Camera photo at {{ selectedLocation.name }}</h2>
    <img :src="cameraScreenshot">
</template>

<script>
export default {
    props:['selectedLocation'],
    data() {
        return {
            cameraScreenshot: null
        }
    },
    created() {
        this.cameraScreenshot = this.selectedLocation.image;
    },
}
</script>