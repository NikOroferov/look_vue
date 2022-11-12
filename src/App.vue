<template>
  <div class="app">
    <h1>Posts page</h1>
    <my-button
      @click="showDialog"
      class="create-post__button"
    >Create new post</my-button>

    <my-dialog v-model:show="dialogVisible">
      <h3>Create post</h3>
      <PostForm
        @create="createPost"
      />
    </my-dialog>

    <PostList
      v-if="!isPostsLoading"
      :posts="posts"
      @remove="removePost"
    />
    <h3 v-else>Loading...</h3>
  </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import axios from 'axios';

export default {
  components: {
    PostForm, PostList
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
    }
  },
  methods: {
    createPost(post) {
      if (post.title !== '' && post.body !== '') {
        this.posts.push(post);
        this.dialogVisible = false;
      }
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;
        const responce = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
        this.posts = responce.data;

        this.isPostsLoading = false;

      } catch (error) {
        alert(`Oooops, we have ${error}`)
      }
    },
  },
  mounted() {
    this.fetchPosts()
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app{
  padding: 15px;
}

.create-post__button{
  margin-top: 15px;
}
</style>