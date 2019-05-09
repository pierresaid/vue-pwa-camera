<template>
  <div class="wrapper">
    <video class="video" ref="video"/>
    <div class="photo-button-container">
      <button class="button photo-button is-medium" @click="TakePhoto">
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
      counter: 0
    };
  },
  methods: {
    async StartRecording() {
      let video = this.$refs.video;
      this.mediaStream = await navigator.mediaDevices.getUserMedia({
        video: true
      });
      video.srcObject = this.mediaStream;
      const mediaStreamTrack = this.mediaStream.getVideoTracks()[0];
      this.imageCapture = new ImageCapture(mediaStreamTrack);
      video.play();
    },
    async TakePhoto() {
      let blob = await this.imageCapture.takePhoto();
      this.photos.push({ id: this.counter++, src: URL.createObjectURL(blob) });
    }
  },
  mounted() {
    this.StartRecording();
  }
};
</script>

<style scoped>
video {
  -webkit-transform: scaleX(-1);
  transform: scaleX(-1);
  height: 100%;
}
.wrapper {
  background-color: black;
  display: grid;
  width: 100vw;
  height: 100vh;
  grid-template-columns: 100%;
  grid-template-rows: [top] 70% [middle] 10% [bottom] 20% [end];
  justify-items: center;
}

.video {
  grid-column: 1;
  grid-row: top / bottom;
}
.photo-button-container {
  grid-column: 1;
  grid-row: middle / bottom;
  z-index: 5;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
}

.photo-button {
  border-radius: 100%;
  color: black;
}

.gallery {
  grid-column: 1;
  grid-row: bottom / end;
}
</style>