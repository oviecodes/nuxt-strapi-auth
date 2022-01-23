<template>
  <div class="w-4/5 mx-auto md:w-1/2 text-center my-12 overflow-hidden">
    <form ref="form" @submit="createPost">
      <h2 class="font-bold text-2xl md:text-4xl mt-5">Create a new post</h2>
      <div>
        <input
          v-model="form.name"
          name="Title"
          type="text"
          placeholder="title"
          class="p-3 my-3 border w-full"
        />
      </div>
      <div>
        <input
          v-model="form.description"
          name="description"
          type="text"
          placeholder="description"
          class="p-3 my-3 border w-full"
        />
      </div>
      <div>
        <textarea
          v-model="form.content"
          name="Content"
          cols="30"
          rows="10"
          class="p-3 my-3 border w-full"
        ></textarea>
      </div>
      <div>
        <input
          type="file"
          name="Image"
          class="p-3 my-3 border w-full"
          @change="assignFileInput()"
        />
      </div>
      <div>
        <button
          class="button--green"
          :disabled="
            form.name === '' ||
            form.description === '' ||
            form.content === '' ||
            fileInput === ''
          "
          type="submit"
        >
          Create
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        name: '',
        description: '',
        content: '',
        users_permissions_user: this.$strapi.user.id,
      },
      fileInput: '',
    }
  },
  methods: {
    async createPost(e) {
      const formData = new FormData()
      let file
      const formElements = this.$refs.form.elements
      formElements.forEach((el, i) => {
        if (el.type === 'file') {
          file = el.files[0]
        }
      })
      formData.append(`files.Image`, file, file.name)
      formData.append('data', JSON.stringify(this.form))
      e.preventDefault()
      await this.$strapi.$articles.create(formData)
      this.$nuxt.$router.push('/articles')
    },
    assignFileInput() {
      const formElements = this.$refs.form.elements
      formElements.forEach((el, i) => {
        if (el.type === 'file') {
          this.fileInput = el.files[0] !== undefined ? el.files[0].name : ''
        }
      })
    },
  },
  middleware({ $strapi, redirect }) {
    if (!$strapi.user) {
      redirect('/articles')
    }
  },
}
</script>

<style></style>
