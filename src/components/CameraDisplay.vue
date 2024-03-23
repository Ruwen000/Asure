<template>
    <div class='ui container'>

        <div class="VideoBox">
            <video v-if="!imageData.image" ref="video" class="camera-stream" />

            <img v-else :src="imageData.image" class="camera-stream">
        </div>

        

        <div class="icon-group">
            <button class="capture" @click="captureImage">Take Picture</button>
            
            <button class="clear" @click="clear_images"></button>
        </div>
        <br>
        <hr>
        <div class="GalleryBox">
            <div v-for="(imageData, index) in imageList" :key="index" class="PictureBox">
            <img   :src="imageData.image" class="gallery_image" />
            <button class="clearEinzelneBilder" @click="clear_image(index)">X</button> 
        </div>
        </div>
        

    </div>
</template>

<script>

import axios from 'axios';

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
                    });

                    const jsonString = JSON.stringify(imageList);
                    console.log(jsonString)

                    const address = 'https://noide.azurewebsites.net/';

                    axios.post(address + 'img', jsonString)
                    .then(response => {
                        console.log('Response:', response.data);
                    })
                    .catch(error => {
                        console.error('Error:', error);
                    });
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

            console.log("testing Server");
            axios.get(address + 'img')
            .then(response => {
                console.log("SERVER IS A BITCH" + response.data);
            })
    },
}
</script>

<style>
    .ui {
        width: 100%;
    }
    .VideoBox{
        display: flex;
        justify-content: center;
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
        max-width: 800px;
        /* width: 100%; */
    }
    .capture {
        background-color: greenyellow;
        border-width: 0px;
        border-radius: px;
        width: 110px;
        height: 40px;
        text-align: center;
    }
    .clear {
        border-width: 0px;
        width:  40px;
        height: 40px;
        text-align: center;
        background-image: url(/Delete.png);
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover;
        border-radius: 20px;
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
    .clearEinzelneBilder{
        position: absolute;
        background-color: red;
        border-width: 0px;
        border-radius: 5px;
        width: 20px;
        height: 20px;
        text-align: center;
    }
</style>
