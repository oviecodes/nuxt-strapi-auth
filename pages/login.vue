<template>
  <div class="w-4/5 mx-auto md:w-1/2 text-center my-12">
    <div v-show="error !== ''" class="p-3 border">
      <p>{{ error }}</p>
    </div>
    <h1 class="font-bold text-2xl md:text-4xl mt-5">Login</h1>
    <form @submit="loginUser">
      <div>
        <input
          v-model="identifier"
          class="p-3 my-5 border w-full"
          type="email"
          placeholder="email"
        />
      </div>
      <div>
        <input
          v-model="password"
          class="p-3 my-5 border w-full"
          type="password"
          placeholder="password"
        />
      </div>
      <div>
        <button
          :disabled="identifier === '' || password === ''"
          class="button--green"
          type="submit"
        >
          Login
        </button>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      identifier: '',
      password: '',
      error: '',
    }
  },
  methods: {
    async loginUser(e) {
      e.preventDefault()
      try {
        const user = await this.$strapi.login({
          identifier: this.identifier,
          password: this.password,
        })
        console.log(user)
        if (user !== null) {
          this.error = ''
          this.$nuxt.$router.push('/articles')
        }
      } catch (error) {
        this.error = 'Error in login credentials'
      }
    },
  },
  middleware: 'authenticated',
}
</script>
<style></style>
