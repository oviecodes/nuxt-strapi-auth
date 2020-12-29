<template>
  <div
    class="flex space-x-5 items-center justify-center bg-black text-white py-3 sm:py-5"
  >
    <NuxtLink to="/articles">Articles</NuxtLink>
    <div v-if="$strapi.user === null">
      <NuxtLink class="border-r px-3" to="/login">Login</NuxtLink>
      <NuxtLink class="border-r px-3" to="/signup">Signup</NuxtLink>
    </div>
    <div v-if="$strapi.user !== null">
      <span class="border-r px-3">{{ $strapi.user.username }}</span>
      <NuxtLink class="border-r px-3" to="/new">Create Post</NuxtLink>
      <button class="pl-3" @click="logout">Logout</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Nav',
  methods: {
    async logout() {
      await this.$strapi.logout()
      this.$nuxt.$router.push('/')
    },
  },
}
</script>

<style></style>
