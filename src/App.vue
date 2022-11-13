<template>
  <div class="app">
    <h1>Posts page</h1>
    <my-input
      v-model:value="searchQuery"
      placeholder="Search post by title..."
    />
    <div class="app__buttons">
      <my-button
        @click="showDialog"
        class="create-post__button"
      >Create new post</my-button>
      <my-select
        v-model="selectedSort"
        :options="sortOption"
      />
    </div>

    <my-dialog v-model:show="dialogVisible">
      <h3>Create post</h3>
      <PostForm
        @create="createPost"
      />
    </my-dialog>

    <PostList
      v-if="!isPostsLoading"
      :posts="sortedAndSearchedPosts"
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
      selectedSort: '',
      sortOption: [
        {value: 'title', name: 'Sort by title'},
        {value: 'body', name: 'Sort by description'},
      ],
      searchQuery: '',
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
  },
  computed: {
    sortedPost() {
      return [...this.posts].sort((a, b) => {
        return a[this.selectedSort]?.localeCompare(b[this.selectedSort]);
      })
    },
    sortedAndSearchedPosts() {
      return this.sortedPost.filter(post => post.title.includes(this.searchQuery))
    }
  },
  watch: {

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

.app__buttons{
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
}
</style>