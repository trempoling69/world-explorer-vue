<template>
  <iframe
    :src="mapSrc"
    frameborder="0"
    scrolling="no"
    marginheight="0"
    marginwidth="0"
    class="map-container"
  ></iframe>
</template>
<script>
export default {
  name: "EmbeddedMap",
  props: {
    query: {
      type: String,
      default: "Place Bellecour, Lyon, France",
    },
    zoom: {
      type: Number,
      default: 2,
    },
  },
  computed: {
    mapSrc: function () {
      return `https://maps.google.com/maps?q=${this.query}&t=&z=${this.zoom}&ie=UTF8&iwloc=&output=embed`;
    },
  },
};
</script>
<style scoped>
.map-container {
  width: 100%;
  height: 500px;
}
</style>