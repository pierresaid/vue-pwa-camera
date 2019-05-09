<template>
  <transition-group class="photos-container" name="list-animated">
    <photo
      class="list-animated-item"
      v-for="photo in photos"
      :key="photo.id"
      :src="photo.src"
      v-on:remove="photos.splice(photos.findIndex(p => p.id === photo.id), 1)"
    />
  </transition-group>
</template>

<script>
import Photo from "./Photo.vue";
export default {
  components: { Photo },
  props: {
    photos: {
      type: Array,
      default: () => {
        return [];
      }
    }
  }
};
</script>

<style scoped>
.photos-container {
  width: 100%;
  height: 100%;
  display: flex;
  overflow-x: scroll;
  overflow-y: hidden;
  background-color: rgb(20, 20, 20);
  user-select: none;
}

.list-animated-item {
  transition: opacity 0.5s;
}
.list-animated-enter {
  opacity: 0;
  /* transform: translateX(30px); */
}
/* .list-animated-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.list-animated-leave-active {
  position: absolute;
  min-width: 1000%;
  height: 20%;
} */
</style>