<template>
  <div>
    <template v-if="size === 'image-full-width'">
      <div class='blog-header single' :style="{ backgroundImage }">
        <template v-if="$prismic.asText(caption) != ''">
          <div class="wrapper">
            <h1>{{ $prismic.asText(caption) }}</h1>
          </div>
        </template>
      </div>
    </template>
    <template v-else>
      <div class='post-part single container'>
        <p class="block-img" :class="size">
          <prismic-image :field="img"/>
        </p>
        <template v-if="$prismic.asText(caption) != ''">
          <p>
            <span class="image-label">{{ $prismic.asText(caption) }}</span>
          </p>
        </template>
      </div>
    </template>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'
import type { Slice, RichTextField, ImageField } from '@prismicio/types'

export default defineComponent({
  name: 'image-caption-slice',
  props: {
    slice: {
      type: Object as PropType<Slice>,
      required: true,
    }
  },
  setup(props) {
    const img = props.slice.primary.image as ImageField
    return {
      img,
      backgroundImage: img.url ? `url(${img.url})` : '',
      caption: props.slice.primary.caption as RichTextField,
      size: props.slice.slice_label as string,
    }
  },
})
</script>

<style scoped>
.blog-header {
  height: 400px;
  position: relative;
  font-family: 'Lato', sans-serif;
  font-weight: 400;
  background-color: white;
  background-size: cover;
  color: white;
  margin-bottom: 3rem;
}
.blog-header::before {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.2);
}
.blog-header::after {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: -moz-linear-gradient(to bottom, rgba(255,255,255,0) 0%, rgba(0,0,0,0) 1%, rgba(0,0,0,0.05) 80%, rgba(0,0,0,0.1) 90%, rgba(0,0,0,0.2) 100%);
  background: -webkit-linear-gradient(to bottom, rgba(255,255,255,0) 0%, rgba(0,0,0,0) 1%, rgba(0,0,0,0.05) 80%, rgba(0,0,0,0.1) 90%, rgba(0,0,0,0.2) 100%);
  background: -o-linear-gradient(to bottom, rgba(255,255,255,0) 0%, rgba(0,0,0,0) 1%, rgba(0,0,0,0.05) 80%, rgba(0,0,0,0.1) 90%, rgba(0,0,0,0.2) 100%);
  background: -ms-linear-gradient(to bottom, rgba(255,255,255,0) 0%, rgba(0,0,0,0) 1%, rgba(0,0,0,0.05) 80%, rgba(0,0,0,0.1) 90%, rgba(0,0,0,0.2) 100%);
  background: linear-gradient(to bottom, rgba(255,255,255,0) 0%, rgba(0,0,0,0) 1%, rgba(0,0,0,0.05) 80%, rgba(0,0,0,0.1) 90%, rgba(0, 0, 0, 0.2) 100%);
}
.blog-header .wrapper {
  text-align: center;
  position: absolute;
  left: 50%;
  top: 50%;
  -ms-transform: translate(-50%, -50%);
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}
img {
  width: 100%;
  height: auto;
}
.block-img {
  margin-bottom: 24px;
}
.image-label {
  display: block;
  text-align: center;
  font-style: italic;
  font-size: 14px;
  color: #949494;
}
/* Media Queries */
@media (max-width: 767px) {
  .blog-header {
    padding: 5px;
  }
  .blog-header .wrapper {
    width: 80%;
  }
}
@media screen and (min-width: 768px) {
  /* Blog post images */
  .block-img.emphasized {
    width: 130%;
    margin: 0 -15% 2rem -15%;
  }
}
</style>