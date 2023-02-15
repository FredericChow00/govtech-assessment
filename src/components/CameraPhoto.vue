<template>
    <div class="cameraphoto">
        <h2>Traffic Camera photo at {{ selectedLocation.name }}</h2>
        <img class="camera-image" :src="cameraScreenshot">
    </div>
</template>

<script>
export default {
    name: 'CameraPhoto',
    props:['selectedLocation'],
    data() {
        return {
            cameraScreenshot: null
        }
    },
    created() {
        this.cameraScreenshot = this.selectedLocation.image;
    },
    watch: {
        selectedLocation: function() {
            this.cameraScreenshot = this.selectedLocation.image;
        }
    }
};
</script>

<style scoped>
.cameraphoto {
	top: 500px;
	left: 20px;
    width: 65%;
	background: lightgrey;
	display: inline-block;
	padding: 20px;
	margin: 10px auto;
	border-radius: 10px;
}
.camera-image {
    max-width: 100%;
    max-height: 100%;
    width: auto;
    height: auto;
    padding: 10px;
}
</style>