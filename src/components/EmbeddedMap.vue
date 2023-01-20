<template>
  <div>
    <MapTypeSelector :selectedType="selectedType" @typeChange="handleChangeType"/>
    <iframe
    :src="mapSrc"
    frameborder="0"
    scrolling="no"
    marginheight="0"
    marginwidth="0"
    class="map-container"
  ></iframe>
  </div>
  
</template>
<script>
import MapTypeSelector from "./MapTypeSelector.vue";
export default {
  name: "EmbeddedMap",
  components: {
   MapTypeSelector
  },
  props: {
    query: {
      type: String,
      default: "Place Bellecour, Lyon, France",
    },
    zoom: {
      type: Number,
      default: 2,
    },
    selectedType : {
      type: String,
      default : 'm'
    }
  },
  computed: {
    mapSrc: function () {
      return `https://maps.google.com/maps?q=${this.query}&t=${this.selectedType}&z=${this.zoom}&ie=UTF8&iwloc=&output=embed`;
    },
  },
  methods:{
    handleChangeType : function (newType){
      this.$emit("typeChange", newType)
    }
  }
};
</script>
<style scoped>
.map-container {
  width: 100%;
  height: 500px;
}
</style>