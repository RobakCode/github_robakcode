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
  },
  apollo: {
    attachments: {
      query: require('./thumbnailElastic.gql'),
      skip() {
        return this.blockAttrs.id === null && !process.client;
      },
      variables() {
        return {
          id:
            this.blockAttrs && this.blockAttrs.imageSourceMap
              ? parseInt(this.blockAttrs.imageSourceMap)
              : this.blockAttrs.id,
        };
      },
      update({ queryPost }) {
        const metas = (queryPost || {}).post_meta || {};
        this.location = metas.location;
        this.fullLocation = metas.fullLocation;
        return Object.values((metas._wp_attachment_metadata || {}).sizes || {}) || [];
      },
    },
  },
  methods: {
    generateSrcset(data = [], imageWidth) {
      return data.length > 0
        ? data
            .filter(({ width }) => Math.abs(width - imageWidth) < 400)
            .map(({ file, width }) => ${this.location}${file} ${width}w)
        : this.fullLocation;
    },
  }
}
</script>
