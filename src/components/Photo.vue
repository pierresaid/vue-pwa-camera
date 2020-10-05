<template>
  <img :src="src" />
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      default: null,
    },
  },
};
</script>

<style scoped>
img {
  height: 100%;
}
</style>