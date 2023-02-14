<template>
    <div>
        <h2>Traffic Camera photo at {{ selectedLocation.name }}</h2>
        <img :src="cameraScreenshot">
    </div>
</template>

<script>
export default {
    props:['selectedLocation'],
    data() {
        return {
            // jpg image of camera screenshot
            cameraScreenshot: undefined,
        }
    },
    computed: {
        getScreenshot() {
            this.cameraScreenshot = this.selectedLocation.image;
        }
    },
}
</script>
    