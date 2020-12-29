<template>
  <div>
    <Nav class="mx-auto sticky top-0" />
    <h1 class="text-center my-5">All our articles</h1>
    <div
      v-for="(article, i) in data"
      :key="i"
      class="sm:flex sm:space-x-5 my-5 shadow-lg mx-auto w-4/5 sm:w-4/5 md:w-4/5 lg:w-1/2"
    >
      <img
        :src="`http://localhost:1337${article.image.url}`"
        class="max-h-screen sm:h-48"
      />
      <div class="px-2 sm:pr-2 sm:text-left text-center">
        <h3 class="font-bold my-3">{{ article.Title }}</h3>
        <p class="my-3">{{ article.description }}</p>
        <button class="button--green mb-4 sm:mb-0" @click="readPost(article)">
          Read more
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  async asyncData({ $strapi, $md }) {
    const data = await $strapi.$articles.find()
    return { data }
  },
  data() {
    return {
      error: '',
    }
  },
  methods: {
    readPost(article) {
      if (this.$strapi.user) {
        this.error = ''
        this.$nuxt.$router.push(`/article/${article.id}`)
      } else {
        this.error = 'please Login to read articles'
        alert(this.error)
      }
    },
  },
}
</script>
<style></style>
