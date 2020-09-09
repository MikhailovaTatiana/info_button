<template>
  <div>
    <ul class="info">
      <li v-for="(value, name, index) in movie" :key="index">{{ name }}: {{ value }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Description",
  props: {
    movie: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.info {
  border-color: blue;
}
</style>