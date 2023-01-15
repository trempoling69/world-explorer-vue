<template>
  <embedded-image :src="country.flags.png"></embedded-image>
</template>
<script>
import EmbeddedImage from "./EmbeddedImage.vue";

export default {
  name: "CountryFlag",

  props: {
    country: {
      type: Object,
      default: null,
    },
  },
  components: { EmbeddedImage },
};
</script>