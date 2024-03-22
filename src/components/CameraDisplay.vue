<template>
    <div class='ui container'>

        <video v-if="!imageData.image" ref="video" class="camera-stream" />

        <img v-else :src="imageData.image" class="camera-stream">

        <div class="icon-group">
            <button class="capture" @click="captureImage">Take Picture</button>
            
            <button class="clear" @click="clear_images">Lösche ALLES</button>
        </div>
        <div class="GalleryBox">
            <div v-for="(imageData, index) in imageList" :key="index" class="PictureBox">
            <img   :src="imageData.image" class="gallery_image" />
            <button class="cleareinzelneBilder" @click="clear_image(index)">X</button> 
        </div>
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

            this.imageList = [];
        },
        clear_image(i){
            this.imageList.splice(i,1);
            console.log(i);
        }
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
        background-color: rgb(211, 80, 40);
        border-width: 0px;
        border-radius: 3px;
        width:  100px;
        height: 20px;
        text-align: center;
    }
    .GalleryBox{
        display: flex;
        flex-wrap: wrap;
    }
    .PictureBox {
        width: 30%;
        display: flex;
        flex-wrap: wrap;
        margin: 1.5%;

    }
    .gallery_image {
        width: 100%;

    }
    .cleareinzelneBilder{
        position: absolute;
        background-color: red;
        border-width: 0px;
        border-radius: 3px;
        width: 20px;
        height: 20px;
        text-align: center;
        margin-left: 21%;
        margin-top: 15%;
    }
</style>
