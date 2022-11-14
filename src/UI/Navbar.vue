<template>
  <div class="navbar">
    <div @click="$router.push('/')">PostApp</div>
    <div class="navbar__buttons">
      <my-button
        @click="$router.push('/posts')"
      >Posts</my-button>
      <my-button
        @click="$router.push('/about')"
      >About</my-button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'navbar'
  }
</script>

<style scoped>
.navbar{
  height: 50px;
  background-color: wheat;
  box-shadow: 2px 2px 4px gray;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 15px;
}

</style>