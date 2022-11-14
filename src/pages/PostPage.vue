<template>
  <div class="post-page">
    <h1>Posts page</h1>
    <my-input v-model:value="searchQuery" placeholder="Search post by title..." />
    <div class="app__buttons">
      <my-button @click="showDialog" class="create-post__button">Create new post</my-button>
      <my-select v-model="selectedSort" :options="sortOption" />
    </div>

    <my-dialog v-model:show="dialogVisible">
      <h3>Create post</h3>
      <PostForm @create="createPost" />
    </my-dialog>

    <PostList v-if="!isPostsLoading" :posts="sortedAndSearchedPosts" @remove="removePost" />
    <h3 v-else>Loading...</h3>

    <div ref="observer" class="observer"></div>

    <!-- <div class="page__wrapper">
      <div
        class="page"
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        :class="{
          'current-page': page === pageNumber
        }"
        @click="changePage(pageNumber)"
      >{{pageNumber}}</div>
    </div> -->
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
        { value: 'title', name: 'Sort by title' },
        { value: 'body', name: 'Sort by description' },
      ],
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
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
    // changePage(pageNumber) {
    //   this.page = pageNumber;
    // },
    async fetchPosts() {
      try {
        this.isPostsLoading = true;

        const responce = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(responce.headers['x-total-count'] / this.limit);

        this.posts = responce.data;
      } catch (error) {
        alert(`Oooops, we have ${error}`)
      } finally {
        this.isPostsLoading = false;
      }
    },
    async loadMorePost() {
      try {
        this.page++;

        const responce = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(responce.headers['x-total-count'] / this.limit);

        this.posts = [...this.posts, ...responce.data]
      } catch (error) {
        alert(`Oooops, we have ${error}`)
      }
    },
  },
  mounted() {
    this.fetchPosts()

    const options = {
      rootMargin: '0px',
      threshold: 1.0
    }
    const callback = (entries, observer) => {
      if (entries[0].isIntersecting && this.page < this.totalPages) {
        this.loadMorePost();
      }
    };
    const observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer)
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
    // page() {
    //   this.fetchPosts()
    // }
  }
}
</script>

<style>
.app__buttons {
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
}

.page__wrapper {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  width: 30%;
}

.page {
  border: 1px solid teal;
  padding: 10px 15px;
  margin-right: 5px;
  cursor: pointer;
}

.page:hover,
.page:focus {
  background-color: whitesmoke;
  border: 1px solid teal;
}

.current-page {
  background-color: ghostwhite;
  color: teal;
}

.observer {}
</style>