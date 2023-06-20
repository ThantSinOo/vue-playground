<template>
    <div class="container">
      <div class="col-6 text-center video_wrapper" :class="{ showPreview: isPreviewVisible }">
        <video ref="videoElement" autoplay style="max-width: 50%;" class="video" v-show="isPreviewVisible"></video>
      </div>
      <canvas ref="canvasElement" style="display: none;"></canvas>
      <button class="btn bg-info" @click="isPreviewVisible = true" v-show="!isPreviewVisible">Click to Capture</button>
      <button v-if="isPreviewVisible && !capturedImage" @click="captureImage" class="btn">Capture</button>
  
      <div v-if="capturedImage">
        <h3>Preview:</h3>
        <img :src="capturedImage" alt="Captured Image" />
        <button @click="retakeImage">Retake</button>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, watchEffect } from 'vue';
  
  export default {
    name: 'ImageCapture',
    setup() {
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

</style>
  