<template>
  <form class="form" @submit.prevent>
    <my-input
      v-model:value.trim="post.title"
      placeholder="title"
      type="text"
      />
      <my-input
      v-model:value.trim="post.body"
      placeholder="description"
      type="text"
    />
    <my-button
      class="form__button"
      @click="createPost"
    >
      Create
    </my-button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now()
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: '',
      }
    },
  }
}
</script>

<style lang="css" scoped>
.form {
  display: flex;
  flex-direction: column;
}

.form__button{
  align-self: flex-end;
}
</style>