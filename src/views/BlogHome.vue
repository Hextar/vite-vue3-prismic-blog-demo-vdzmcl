<template>
  <div v-if="hasContent" class="page">
    <div class="home">
      <div class="blog-avatar" :style="{ backgroundImage }">
      </div>

      <h1 class="blog-title">
        {{ fields.headline }}
      </h1>
      <p class="blog-description">{{ fields.description }}</p>
    </div>
    <blog-posts/>
  </div>

  <div v-else class="home">
    <p>Please add some content to your blog home document.</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref, onMounted, computed } from 'vue'
import BlogPosts from '@/components/BlogPosts.vue'
import type { RichTextField, ImageField } from '@prismicio/types'
import { usePrismic } from '@prismicio/vue'
import { useRouter } from 'vue-router'

export default defineComponent({
  name: 'blog-home',
  components: {
    BlogPosts
  },
  setup() {
    const fields = reactive({
      headline: '',
      description: '',
      image: '',
    } as {
      headline: string,
      description: string,
      image: string,
    })
    const hasContent = ref(false)
    const { client, asText } = usePrismic()
    const router = useRouter()

    const checkForContent = () => {
      if (
        fields.image ||
        fields.headline ||
        fields.description
      ) {
        hasContent.value = true
      }
    }

    onMounted(async () => {
      const document = await client.getSingle('blog_home', {})
      if (document) {
        fields.headline = asText(document.data.headline as RichTextField)
        fields.description = asText(document.data.description as RichTextField)
        fields.image = (document.data.image as ImageField)?.url ?? ''
        checkForContent()
      } else {
        router.push({ name: 'not-found' })
      }
    })

    return {
      backgroundImage: computed(() => fields.image ? `url(${fields.image})` : ''),
      hasContent,
      fields,
    }
  }
})
</script>

<style scoped>
.home {
  max-width: 700px;
  margin: auto;
  text-align: center;
}
.home .blog-avatar {
  height: 140px;
  width: 140px;
  border-radius: 50%;
  background-position: center;
  background-size: cover;
  margin: 1em auto;
}
.home .blog-description {
  font-size: 18px;
  color: #9A9A9A;
  line-height: 30px;
  margin-bottom: 3rem;
  padding-bottom: 3rem;
  font-family: 'Lato', sans-serif;
  border-bottom: 1px solid #DADADA;
}
@media (max-width: 767px) {
  .home {
    padding: 0 20px;
  }
}
</style>