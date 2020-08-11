<template>
  <div class="chart">
    <div class="container">
      <polar-area />
    </div>
  </div>
</template>

<script>
import PolarArea from '~/components/PolarArea.vue'

export default {
  components: {
    PolarArea,
  },
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
}
</style>
