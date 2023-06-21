<template>










    <div class="container d-flex justify-content-center">
      <div class="col-6 bg-info photo_upload_container" style="max-width: 350px; position: relative;">
        <button :class="{ 'active': showButtons }" class="btn btn-primary upload_photo_btn" @click="showButtons = true">
          Upload Photo
        </button>
        <!-- Upload From Camera and Upload from Local Element  -->
        <div v-if="showButtons" class="bg-danger overlay  col-12">
          <div class="col-12 btns_container row">
            Upload from Camera Btn 
            <div class="col-6 upload_camera_container">
              <!-- Combination two btns  -->
              <button :class="['btn', 'btn-success', 'upload_camera_btn', { 'active': showNo1 }]" @click="showNo1 = true; isPreviewVisible = true" v-show="!isPreviewVisible">Upload from Camera</button>
            </div>
            <div class="col-6  upload_local_container">
              <button :class="{ 'active': showNo2 }" class=" btn btn-warning upload_local_btn" @click="showNo2 = true" >
              Upload from Loacl Storage
              </button>
            </div>
          </div>
        </div>

        <!-- element that will popup if click Upload From Camera  -->
        <div v-if="showNo1" class="bg-success live_preview_container">
          <div class="live_preview_container">
            <div class="col-12 camera_container">
              <div class="col-6 text-center video_wrapper" :class="{ showPreview: isPreviewVisible }">
                <video ref="videoElement" autoplay style="max-width: 50%;" class="video" v-show="isPreviewVisible"></video>
              </div>
              <canvas ref="canvasElement" style="display: none;"></canvas>
            </div>
            <div class="col-12 capture_btn_container">
              <button :class="{ 'active': showNo3 }" class="btn btn-danger" @click="showNo3 = true">Capture</button>
            </div>
          </div>
        </div>
      </div>




      <!-- raws btn for line 22  -->
       <!-- <button :class="{ 'active': showNo1 }" class=" btn btn-success upload_camera_btn" @click="showNo1 = true" class="btn bg-info" @click="isPreviewVisible = true" v-show="!isPreviewVisible">Upload from Camera</button>
            <button class="btn bg-info" @click="isPreviewVisible = true" v-show="!isPreviewVisible">Upload from Camera</button> -->
           
            <button :class="{ 'active': showNo3 }" class="btn btn-danger" @click="showNo3 = true">Capture</button>



      <div class="col-6 text-center video_wrapper" :class="{ showPreview: isPreviewVisible }">
        <video ref="videoElement" autoplay style="max-width: 50%;" class="video" v-show="isPreviewVisible"></video>
      </div>
      <canvas ref="canvasElement" style="display: none;"></canvas>
      <div class="button_container">
        <div>
          <button class="btn bg-success" @click="photo_active">Upload Photo</button>
        </div>
        <div class="" :class="{  is_choice:is_choice_option_visible }">
          <div>
            <button class="btn bg-info" @click="isPreviewVisible = true" v-show="!isPreviewVisible">Upload from Camera</button>
          </div>
          <div>
            <button>Upload Photo</button>
          </div>
        </div>
      </div>
      <button v-if="isPreviewVisible && !capturedImage" @click="captureImage" class="btn">Capture</button>
  
      <div class="capturedImageGallery_wrapper">
        <div v-if="capturedImage" class="capturedImageGallery">
        <h3>Preview:</h3>
        <img :src="capturedImage" alt="Captured Image" />
        <button @click="retakeImage">Retake</button>
      </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, watchEffect } from 'vue';
  
  export default {
    name: 'ImageCapture',
    setup() {
      const showButtons = ref(false);
      const showNo1 = ref(false);
      const showNo2 = ref(false);
      const showNo3 = ref(false);
     
  
      const cancelNo3 = () => {
        showNo3.value = false;
        showNo4.value = false;
        showNo5.value = false;
      };
  
      const submitNo4 = () => {
        // Handle submission logic
        showNo3.value = false;
        showNo4.value = false;
        showNo5.value = false;
        console.log("No4 worked");
      };

      const videoElement = ref(null);
      const canvasElement = ref(null);
      const capturedImage = ref(null);
      const stream = ref(null);
      const isPreviewVisible = ref(false);
  
      const setupCamera = async () => {
        try {
          stream.value = await navigator.mediaDevices.getUserMedia({
            video: true,
            audio: false,
          });
          videoElement.value.srcObject = stream.value;
        } catch (error) {
          console.error('Error accessing camera:', error);
        }
      };
  
      const captureImage = () => {
        const video = videoElement.value;
        const canvas = canvasElement.value;
        const context = canvas.getContext('2d');
  
        canvas.width = video.videoWidth;
        canvas.height = video.videoHeight;
  
        context.drawImage(video, 0, 0, canvas.width, canvas.height);
        capturedImage.value = canvas.toDataURL('image/png');
      };
  
      const retakeImage = () => {
        capturedImage.value = null;
        videoElement.value.srcObject = stream.value; // Reset the camera stream
        isPreviewVisible.value = false;
      };
  
      watchEffect(() => {
        if (videoElement.value) {
          videoElement.value.srcObject = stream.value;
        }
      });
  
      return {
        showButtons,
        showNo1,
        showNo2,
        showNo3,
        cancelNo3,
        submitNo4,


        videoElement,
        canvasElement,
        capturedImage,
        isPreviewVisible,
        setupCamera,
        captureImage,
        retakeImage,
      };
    },
    mounted() {
      this.setupCamera();
    },
    beforeUnmount() {
      if (this.stream) {
        const tracks = this.stream.getTracks();
        tracks.forEach((track) => track.stop());
      }
    },
  };
  </script>

  <style>

.container{
    margin-top: 10%;
  }

  .photo_upload_container{
    height: 300px;
    width: 300px;

    border-radius: 15px;

    display: flex;
    justify-content: center;
    align-items: center;

    position: relative;
  }

  .upload_photo_btn{
    
  }
  .overlay {
    width: 300px;
    height: 300px;
    position: relative;

    position :absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 99;
  }
  .btns_container{
    position :absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .btns_container > div > button{
    width: 100px;
    height: 50px;
    font-size: 12px;
  }
  .overlay > button{
    font: 10px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .live_preview_container{
    width: 350px;
    height: 350px;
    color: blueviolet;
    border-radius: 15px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    z-index: 199;
  }

  .camera_container{
    height: 80%;
  }
  
  .popup {
    position: absolute;
    top: 0;
    left: 0;
    border: 1px solid #ccc;
    padding: 10px;
    margin-top: 10px;
  }
  
  .active {
    background-color: yellow;
  }
  /* .container{
    display: flex;
    justify-content: center;
    align-items: center;
  }
    .video_wrapper{
        border-radius: 15px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .video{
        border-radius: 15px;
    }

    .capturedImageGallery{
        
    } */

</style>
  