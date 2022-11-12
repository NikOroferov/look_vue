<template>
  <div class="post">
    <div>
      <div>
        <strong>Title: {{ post.title }}</strong>
      </div>
      <div>
        <strong>Description: {{ post.body }}</strong>
      </div>
    </div>
    <div class="post__buttons">
      <my-button
        @click="$emit('remove', post)"
      >
        Delete
      </my-button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  }
}
</script>

<style scoped>
.post {
  display: flex;
  align-items: center;
  justify-content: space-between;

  padding: 15px;
  border: 2px solid teal;
  margin-bottom: 15px;
}
</style>