<template>
    <div class="cameraphoto p-3 rounded">
        <h4 class="mb-3">Traffic Camera photo at {{ selectedLocation.name }}</h4>
        <img v-if="this.cameraScreenshot" class="cameraimage img-fluid rounded" :src="this.cameraScreenshot">
        <p v-else>Loading traffic camera photo...</p>
    </div>
</template>

<script>
export default {
    name: 'CameraPhoto',
    props:['selectedLocation'],
    data() {
        return {
            cameraScreenshot: undefined
        }
    },
    created() {
        this.cameraScreenshot = this.selectedLocation.image;
    },
    watch: {
        selectedLocation: function() {
            this.cameraScreenshot = undefined;
            this.cameraScreenshot = this.selectedLocation.image;
        }
    }
};
</script>

<style scoped>
.cameraimage {
    width: 100%;
    object-fit: contain;
}
.cameraphoto {
	background: lightgrey;
    border-radius: 10px;
}
</style>