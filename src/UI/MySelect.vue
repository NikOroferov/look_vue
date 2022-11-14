<template>
  <select
    class="select"
    :value="modelValue"
    @change="changeOption"
   >
    <option disabled value="">Select value</option>
    <option
      v-for="option in options"
      :key="option.value"
      :value="option.value"
    >{{option.name}}</option>
  </select>
</template>

<script>
export default {
  name: 'my-select',
  props: {
    modelValue: {
      type: String,
    },
    options: {
      type: Array,
      default: ()=>[],
    }
  },
  methods: {
    changeOption(event) {
      this.$emit('update:modelValue', event.target.value)
    }
  }
}
</script>

<style scoped>
.select{
  border: 2px solid teal;
  width: 150px;
  text-align: center;
  color: teal;
  cursor: pointer;
}

.select:hover,
.select:focus{
  background-color: ghostwhite;
}
</style>