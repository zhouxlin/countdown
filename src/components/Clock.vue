<template>
  <div>
    <h1>{{ clock }}</h1>
  </div>
</template>

<script>
export default {
  name: 'clock',
  props: {
    clock: String
  },
  data () {
    return {
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 160px;
}
</style>
