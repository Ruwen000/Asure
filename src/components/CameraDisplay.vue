<template>
    <div class='ui container'>
        <video v-if="!imageData.image" ref="video" class="camera-stream" />
        <img v-else :src="imageData.image" v-bind:style="{transform: 'rotate(' + main_image_orientation + 'deg'}" class="camera-stream">
        <div class='ui divider'></div>
        <div class="icon-group">   
            <div  class="camera-icon" @click="captureImage">
                <i class="big camera icon" ></i>
                <button>Take Picture</button>
            </div>
            <div class='camera-icon' @click="rotateImage">
                <i class="big redo alternate icon"></i>
                <button>Rotate</button> 
            </div>
            <div class="camera-icon" @click="cancelImage">
                <i class="big cancel icon"></i>
                <button>Cancel</button>
            </div>
        </div>
        <div>
            <!-- Iterate over each image object in imageList -->
            <img v-for="(imageData, index) in imageList" :key="index" :src="imageData.image" class="camera-stream" />
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
                image_orientation:0,
            },

            main_image_orientation: 0,
        }
    },
    methods: {

        captureImage() {
            
            console.log("Taking Image")
            const mediaStreamTrack = this.mediaStream.getVideoTracks()[0]
            const imageCapture = new window.ImageCapture(mediaStreamTrack)
            let reader = new FileReader();
            return imageCapture.takePhoto().then(blob => {

                reader.readAsDataURL(blob)
                reader.onload = () => {
                    this.imageList.push({
                        image: reader.result,
                        image_orientation: main_image_orientation,
                    })
                }
            })  
        },

        rotateImage() {

            // this.main_image_orientation = this.main_image_orientation + 90; 
            this.main_image_orientation += 90; 
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
    .icon-group {
        display: flex;
        flex-direction: row;
        justify-content: space-evenly;
        margin: 12px auto;
    }
    .camera-icon {
        width: 15%;
        vertical-align: middle;
        margin: auto;
    }
    .camera-stream {
        margin: 120px 170px;
        width: 50%;
    }
     
</style>
