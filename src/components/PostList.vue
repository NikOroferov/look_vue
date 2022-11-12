<template>
  <div
    v-if="posts.length > 0"
    class="post__list"
  >
    <h3>Posts list</h3>
    <post-item
      v-for="post in posts"
      :post="post"
      :key="post.id"
      @remove="$emit('remove', post)"
    />
  </div>
  <h2
    v-else
    class="error"
  >Posts list is empty</h2>
</template>

<script>
import PostItem from './PostItem.vue';
export default {
  components: { PostItem },

  props: {
    posts: {
      type: Array,
      required: true,
    }
  }
}
</script>

<style scoped>
.post__list{
  margin-top: 15px;
}

.error{
  color: brown;
}
</style>