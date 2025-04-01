
<template>
  <div class="wildlife-dashboard">
    <div class="dashboard-container">
      <div class="upload-section">
        <h2 class="section-title">🎵 Upload Audio File</h2>
        <div class="file-upload-box">
          <input type="file" @change="handleFileChange" accept="audio/*" class="file-input" />
          <button @click="uploadFile" :disabled="!selectedFile" class="upload-btn">Upload</button>
        </div>
        
        <div v-if="audioUrl" class="audio-player-section">
          <h3 class="section-subtitle">🎧 Uploaded Audio</h3>
          <audio controls class="audio-player">
            <source :src="audioUrl" type="audio/wav" />
            Your browser does not support the audio element.
          </audio>
        </div>

        <div v-if="predictionResult !== null" class="prediction-result">
          <h3 class="section-subtitle">🔍 Prediction Result</h3>
          <p class="result-text">{{ predictionResult }}</p>
          
          <div v-if="imageUrl || serverImageUrl" class="prediction-images">
            <div v-if="imageUrl" class="image-container">
              <h4 class="image-title">Prediction Image:</h4>
              <img :src="imageUrl" alt="Mapped Prediction Image" />
            </div>
            <div v-if="serverImageUrl" class="image-container">
              <h4 class="image-title">Spectrogram Image:</h4>
              <img :src="serverImageUrl" alt="Processed Image" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";


export default {
  data() {
    return {
      selectedFile: null,
      predictionResult: null,
      imageUrl: "",
      serverImageUrl: "",
      audioUrl: "",
      imagesMap: {
        "Fire": "fire.jpg",
        "Rain": "rain.jpg",
        "Thunderstorm": "thunderstorm.jpg",
        "WaterDrops": "waterdrops.jpg",
        "Wind": "wind.jpg",
        "Silence": "silence.jpg",
        "TreeFalling": "treefalling.jpg",
        "Helicopter": "helicopter.jpg",
        "VehicleEngine": "vehicleengine.jpg",
        "Axe": "axe.jpg",
        "Chainsaw": "chainsaw.jpg",
        "Generator": "generator.jpg",
        "Handsaw": "handsaw.jpg",
        "Firework": "firework.jpg",
        "Gunshot": "gunshot.jpg",
        "WoodChop": "woodchop.jpg",
        "Whistling": "whistling.jpg",
        "Speaking": "speaking.jpg",
        "Footsteps": "footsteps.jpg",
        "Clapping": "clapping.jpg",
        "Insect": "insect.jpg",
        "Frog": "frog.jpg",
        "BirdChirping": "birdchirping.jpg",
        "WingFlapping": "wingflapping.jpg",
        "Lion": "lion.jpg",
        "WolfHowl": "wolfhowl.jpg",
        "Squirrel": "squirrel.jpg"
      }
    };
  },
  methods: {
    handleFileChange(event) {
      this.selectedFile = event.target.files[0];
      if (this.selectedFile) {
        this.audioUrl = URL.createObjectURL(this.selectedFile);
      }
    },

    async uploadFile() {
      if (!this.selectedFile) {
        alert("Please select a file first.");
        return;
      }

      const token = localStorage.getItem("token");
      const userId = localStorage.getItem("user_id");

      const formData = new FormData();
      formData.append("file", this.selectedFile);
      formData.append("user_id", userId);

      try {
        const response = await axios.post("http://127.0.0.1:5000/upload", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
            "Authorization": `Bearer ${token}`
          },
        });

        console.log("Response from server:", response.data);

        if (response.data.error) {
          alert(response.data.error);
          return;
        }

        this.predictionResult = response.data.prediction;

        let imageFile = this.imagesMap[this.predictionResult];
        this.imageUrl = imageFile ? `http://127.0.0.1:5000/static/images/${imageFile}` : "";

        this.serverImageUrl = `http://127.0.0.1:5000/get_image?image_path=${encodeURIComponent(response.data.image_path)}`;
      } catch (error) {
        console.error("Error uploading file:", error);
        alert("Failed to upload file.");
      }
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700&display=swap');

body, html {
  margin: 0;
  padding: 0;
  height: 100%;
  background-image: url('./bg5.png');  
  background-position: center;
  background-attachment: fixed;
  font-family: 'Orbitron', sans-serif;
  overflow-x: hidden;
}

.wildlife-dashboard {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-blend-mode: overlay;
  perspective: 1000px;
}

.dashboard-container {
  background: rgba(5, 98, 67, 0.85);
  border-radius: 20px;
  padding: 30px;
  width: 95%;
  max-width: 600px;
  backdrop-filter: blur(15px);
  border: 2px solid rgba(50, 255, 126, 0.2);
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
  transform: rotateX(10deg);
  transition: all 0.3s ease;
}

.section-title, .section-subtitle {
  color: #73f5c3;
  text-shadow: 0 0 10px rgba(50, 255, 126, 0.5);
  text-align: center;
  font-weight: 500;
  letter-spacing: 1px;
}

.file-upload-box {
  background: rgba(0, 50, 30, 0.6);
  border: 2px dashed #32ff7e;
  border-radius: 15px;
  padding: 40px;
  text-align: center;
  transition: all 0.3s ease;
}

.file-input {
  background: rgba(0, 70, 50, 0.5);
  color: #73f5c3;
  border: 2px solid #32ff7e;
  border-radius: 10px;
  padding: 15px;
  width: 100%;
  margin-top: 15px;
  transition: all 0.3s ease;
}

.upload-btn {
  background-color: #32ff7e;
  color: rgba(0, 30, 20, 0.9);
  border: none;
  padding: 12px 25px;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  text-transform: uppercase;
  font-weight: bold;
  letter-spacing: 2px;
  margin-top: 15px;
}

.upload-btn:hover {
  background-color: #00ffab;
  transform: scale(1.05);
  box-shadow: 0 0 15px rgba(50, 255, 126, 0.5);
}

.upload-btn:disabled {
  background-color: rgba(50, 255, 126, 0.5);
  cursor: not-allowed;
}

.audio-player {
  width: 100%;
  margin-top: 15px;
  background: rgba(0, 70, 50, 0.5);
  border-radius: 10px;
}

.prediction-result {
  background: rgba(0, 50, 30, 0.6);
  border-radius: 15px;
  padding: 25px;
  margin-top: 20px;
  border-left: 5px solid #32ff7e;
}

.result-text {
  color: #f5a373;
  text-align: center;
  font-weight: 500;
}

.prediction-images {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.image-container {
  width: 48%;
}

.image-title {
  color: #73f5c3;
  text-align: center;
}

img {
  max-width: 100%;
  border-radius: 10px;
  border: 3px solid #32ff7e;
  box-shadow: 0 0 15px rgba(50, 255, 126, 0.3);
  transition: transform 0.3s ease;
}

img:hover {
  transform: scale(1.02);
}

@media screen and (max-width: 600px) {
  .dashboard-container {
    width: 95%;
    padding: 20px;
    transform: none;
  }
  
  .prediction-images {
    flex-direction: column;
  }
  
  .image-container {
    width: 100%;
    margin-bottom: 15px;
  }
}
</style>