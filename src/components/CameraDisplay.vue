<template>
    <div class='ui container'>
        <video v-if="!imageData.image" ref="video" class="camera-stream" />
        <img v-else :src="imageData.image" class="camera-stream">
        <div class='ui divider'></div>
        <div class="icon-group">
            <button class="capture" @click="captureImage">Take Picture</button>
            <button class="clear" @click="clear_images">X</button>
        </div>
        <div class="gallery_box">
            <img style="background-color: azure;" v-for="(imageData, index) in imageList" :key="index" :src="imageData.image" class="gallery_image" />
        </div>

    </div>
</template>

<script>

export default {

    data() {
        return {
            defaultImage: '../../src/assets/image.png',
            mediaStream: null,
            imageList: [],     // Array to hold multiple images
            imageData: {
                image: ``,
            },

        }
    },
    methods: {

        captureImage() {

            const mediaStreamTrack = this.mediaStream.getVideoTracks()[0]
            const imageCapture = new window.ImageCapture(mediaStreamTrack)
            let reader = new FileReader();
            return imageCapture.takePhoto().then(blob => {

                reader.readAsDataURL(blob)
                reader.onload = () => {
                    this.imageList.push({
                        image: reader.result,
                    })
                }
            })
        },

        cancelImage() {

            this.showCameraModal = true;
            navigator.mediaDevices.getUserMedia({video: true})
            .then(mediaStream => {
                    this.$refs.video.srcObject = mediaStream;
                    this.$refs.video.play()
                    this.mediaStream = mediaStream
            })
        },

        clear_images() {

            console.log("CKJSFHOKSLNBV")
            this.imageList = [];
        },
    },
    mounted() {

        navigator.mediaDevices.getUserMedia({video: true})
            .then(mediaStream => {
                    this.$refs.video.srcObject = mediaStream;
                    this.$refs.video.play()
                    this.mediaStream = mediaStream
            })
    },
}
</script>

<style>
    .ui {
        width: 100%;
    }
    .icon-group {
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
    }
    .camera-icon {
        width: 15%;
        vertical-align: middle;
        margin: auto;
    }
    .camera-stream {
        width: 100%;
        /* width: 100%; */
    }
    .capture {
        border-width: 0px;
        border-radius: 2px;
        width: 110px;
        height: 20px;
        text-align: center;
    }
    .clear {
        background-color: red;
        border-width: 0px;
        border-radius: 3px;
        width: 20px;
        height: 20px;
        text-align: center;
    }
    .gallery_box {
        width: 100%;
        display: flex;
        flex-wrap: wrap;
    }
    .gallery_image {
        width: 30%;
        margin: 1.5%;
    }

</style>
