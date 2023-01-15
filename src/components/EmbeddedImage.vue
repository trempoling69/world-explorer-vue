<template>
  <p class="image is-4by3">
    <img :src="src" alt="" />
  </p>
</template>
<script>
export default {
    name: "EmbeddedImage",
    props: {
        src: {
            type: String,
            default: ""
        }
    }
}
</script>