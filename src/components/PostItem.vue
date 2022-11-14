<template>
  <div class="post">
    <div>
      <strong>#{{post.id}}</strong>
      <div class="title">
        <strong>Title: </strong>
        {{ post.title }}
      </div>
      <div>
        <strong>Description: </strong>
        {{ post.body }}
      </div>
    </div>
    <div class="post__buttons">
      <my-button
        @click="$router.push(`/posts/${post.id}`)"
      >
        Open
      </my-button>
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

.title{
  margin: 10px 0;
}

.post__buttons{
  display: flex;
  flex-direction: column;
}
</style>