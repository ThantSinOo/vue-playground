<template>
    <div class="container d-flex justify-content-center">
      <div class="col-6 bg-info photo_upload_container" style="max-width: 350px; position: relative;">
        <button
          :class="{ 'active': showButtons }"
          class="btn btn-primary upload_photo_btn"
          @click="showButtons = true"
        >
          Upload Photo
        </button>
  
        <div v-if="showButtons" class="bg-danger overlay  col-12">
          <div class="col-12 btns_container row">
            <div class="col-6 upload_camera_container">
            <button
            :class="{ 'active': showNo1 }"
            class=" btn btn-success upload_camera_btn"
            @click="showNo1 = true"
          >
            Upload from Camera
            </button>
            </div>
            <div class="col-6  upload_local_container">
              <button
              :class="{ 'active': showNo2 }"
              class=" btn btn-warning upload_local_btn"
              @click="showNo2 = true"
            >
              Upload from Loacl Storage
              </button>
            </div>
          </div>
        </div>
  
        <div v-if="showNo1" class="bg-success live_preview_container">
          <div class="live_preview_container">
            <div class="col-12 camera_container">Camera</div>
            <div class="col-12 capture_btn_container">
              <button
            :class="{ 'active': showNo3 }"
            class="btn btn-danger"
            @click="showNo3 = true"
          >
            Capture
          </button>
            </div>
          </div>
        </div>
  
        <div v-if="showNo3" class="overlay">
          <div class="popup" style="z-index: 2;">
            <p>Picture Container</p>
            <button class="btn btn-primary" @click="showNo4 = true">Submit</button>
            <button class="btn btn-secondary" @click="cancelNo3()">Cancel</button>
          </div>
        
        </div>
  
        
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  
  export default {
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

      const camera_or_local_popup = () =>{

      }
  
      return {
        showButtons,
        showNo1,
        showNo2,
        showNo3,
       
        cancelNo3,
        submitNo4
      };
    }
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
  </style>
  