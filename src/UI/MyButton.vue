<template>
  <button class="button">
    <slot></slot>
  </button>
</template>

<script>
  export default {
    name: 'my-button'
  }
</script>

<style scoped>
.button {
  padding: 10px 15px;
  color: teal;
  background-color: transparent;
  border: 2px solid teal;
  cursor: pointer;
}

.button:hover, .button:focus {
  background-color: whitesmoke;
}
</style>