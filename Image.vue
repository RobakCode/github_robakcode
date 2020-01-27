<template>
  <figure>
    <h1>Img</h1>
    <h2>{{ blockAttrs }}</h2>
    <img src="" alt="">
  </figure>
</template>

<script>
export default {
  props: {
    blockAttrs: {
      type: Object,
      default: () => ({})
    },
    innerHtml: {
      type: String,
      default: ''
    }
  }
}
</script>
