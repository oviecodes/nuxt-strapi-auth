<template>
  <div>
    <Nav class="mx-auto sticky top-0" />
    <div class="w-4/5 sm:w-1/2 mx-auto my-5">
      <h3 class="my-5 font-bold text-4xl">{{ article.Title }}</h3>
      <img
        :src="`http://localhost:1337${article.image.url}`"
        class="max-h-screen"
      />
      <p class="mt-5 font-bold">
        written by {{ article.users_permissions_user.username }}
      </p>
      <div class="my-5" v-html="$md.render(article.Content)"></div>
      <button
        v-if="
          $strapi.user && article.users_permissions_user.id === $strapi.user.id
        "
        class="button--grey"
        @click="deletePost(article.id)"
      >
        Delete
      </button>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $strapi, route }) {
    const id = route.params.id
    const article = await $strapi.$articles.findOne(id)
    return { article }
  },
  methods: {
    async deletePost(id) {
      await this.$strapi.$articles.delete(id)
      this.$nuxt.$router.push('/articles')
    },
  },
  middleware({ $strapi, redirect }) {
    if ($strapi.user === null) {
      redirect('/articles')
    }
  },
}
</script>

<style scoped>
h1 {
  font-weight: 700;
  font-size: 2rem;
  margin: 0.5em 0;
}
</style>
