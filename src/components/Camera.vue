<template>
  <div class="wrapper">
    <video class="video" :class="activeDevice === 0 ? 'front' : ''" ref="video"/>

    <button
      v-if="videoDevices.length > 1"
      class="button is-rounded is-outlined switch-button"
      @click="switchCamera"
    >
      <b-icon pack="fas" icon="sync-alt"/>
    </button>
    <div class="photo-button-container">
      <button class="button photo-button" @click="TakePhoto">
        <b-icon pack="fas" icon="camera"/>
      </button>
    </div>
    <photos-gallery class="gallery" :photos="photos"/>
  </div>
</template>



<script>
import PhotosGallery from "./PhotosGallery.vue";
export default {
  components: {
    PhotosGallery
  },
  data() {
    return {
      photos: [],
      mediaStream: null,
      videoDevices: [],
      activeDevice: 0,
      counter: 0
    };
  },
  methods: {
    async StartRecording(deviceIdx) {
      let video = this.$refs.video;
      this.mediaStream = await navigator.mediaDevices.getUserMedia({
        video: { deviceId: { exact: this.videoDevices[deviceIdx].deviceId } }
      });
      video.srcObject = this.mediaStream;
      const mediaStreamTrack = this.mediaStream.getVideoTracks()[0];
      this.imageCapture = new ImageCapture(mediaStreamTrack);
      video.play();
    },
    async TakePhoto() {
      let blob = await this.imageCapture.takePhoto();
      console.log(blob);

      this.photos.push({ id: this.counter++, src: URL.createObjectURL(blob) });
    },
    switchCamera() {
      const tracks = this.mediaStream.getVideoTracks();
      tracks.forEach(track => {
        track.stop();
      });
      this.StartRecording((this.activeDevice + 1) % 2);
      this.activeDevice = (this.activeDevice + 1) % 2;
    }
  },
  async mounted() {
    const devices = await navigator.mediaDevices.enumerateDevices();
    this.videoDevices = devices.filter(d => d.kind === "videoinput");
    this.StartRecording(0);
  }
};
</script>

<style scoped>
.video.front {
  -webkit-transform: scaleX(-1);
  transform: scaleX(-1);
}
.wrapper {
  background-color: black;
  display: grid;
  width: 100vw;
  height: 100vh;
  grid-template-columns: [left] 90vw [bs] 5vw [es] 5vw [right];
  grid-template-rows: [top] 5vh [bs] 5vh [es] 60vh [middle] 10vh [bottom] 20vh [end];
  justify-items: center;
  overflow: hidden;
}

.video {
  height: 100%;
  grid-column: left/right;
  grid-row: top / bottom;
  user-select: none;
  max-width: unset;
}

.switch-button {
  grid-column: bs / es;
  grid-row: bs / es;
  z-index: 5;
  border-radius: 100%;
  width: 6vh;
  height: 6vh;
  font-size: 2vh;
}

.photo-button-container {
  grid-column: left / right;
  grid-row: middle / bottom;
  z-index: 5;
  width: 100vw;
  height: 20vh;
  display: flex;
  justify-content: center;
}

.photo-button {
  width: 10vh;
  height: 10vh;
  border-radius: 100%;
}

.photo-button {
  font-size: 4vh;
  color: black;
}
.gallery {
  grid-column: left / right;
  grid-row: bottom / end;
}
</style>