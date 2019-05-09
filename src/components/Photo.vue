<template>
  <div class="wrapper">
    <i class="remove icon fas fa-times" @click="$emit('remove')"/>
    <a class="save" :href="src" download>
      <i class="icon fas fa-save"/>
    </a>

    <img :src="src">
  </div>
</template>

<script>
export default {
  props: {
    src: {
      type: String,
      default: null
    }
  },
  methods: {
    download() {}
  }
};
</script>

<style scoped>
.wrapper {
  display: grid;
  grid-template-columns: [b] 70% [bs] 5% [es] 10% [br] 10% [er] 5% [e];
  grid-template-rows: [b] 5% [br] 10% [er] 85% [e];
  align-items: baseline;
  min-width: fit-content;
}

.icon {
  font-size: 2.5vh;
}

.remove {
  grid-column: br/er;
  grid-row: br/er;
  z-index: 2;
  color: rgb(223, 73, 73);
  cursor: pointer;
}

.save {
  grid-column: bs/es;
  grid-row: br/er;
  z-index: 2;
  color: white;
  cursor: pointer;
}

img {
  height: 100%;
  -webkit-transform: scaleX(-1);
  transform: scaleX(-1);
  grid-column: b / e;
  grid-row: b/e;
}
</style>